---
layout: post
title: "Vertica: Query Execution"
date: 2020-08-28
categories: [knowledge, vertica]
tags: [vertica9.x]
---

# Vertica: Database Epochs

## Vertica Epoch Model

An epoch is a logical unit of time in which a single change is made to the data in the system. This unit is represented by a 64 bit number. With each commit, the epoch number increases and the data added to Vertica is stamped with the current epoch.

Epochs are used in Vertica to establish points in time to allow for historical queries - a point across nodes to which a database can be reset and a point at which data marked with *delete* vectors can be permanently purged from the database/

Vertica supports the following:

- **Current Epoch *CE***: Where data is currently being written - contains all uncommited changes.
- **Latest Epoch *LE***: The point at which the most recent database commit was completed
- **Last Good Epoch *LGE***: Most recent epoch to which the database can be rolled back in the case of failure. This is the last point at which the database was running on all the nodes.
- **Ancient History Mark *AHM***: Marks the point in time at which data marked for deletion can be purged from the database.  Epochs occuring later than the AHM epoch can be used in queries. Their data cannot be physically purged. You can not perform historical queries at epochs earlier than the AHM. By default, resets to the LGE every 3 minutes. To manually set the AHM to the LGE, run the command `\set make AHM now`.

## Identifying Epochs

Epochs are found in the `SYSTEM` System Table. For example:

```sql
SELECT current_epoch, ahm_epoch, last_good_epoch FROM SYSTEM;
```

The epochs can also be found using the following database functions:

- `SELECT GET_CURRENT_EPOCH();`
- `SELECT GET_AHM_EPOCH();`
- `SELECT GET_LAST_GOOD_EPOCH();`

The `EPOCHS` table shows the epochs that can be queried. For example:

```sql
SELECT * FROM EPOCHS;
```

## Epoch Configuration Parameters

| Parameter Name             	| Description                                                                                              	| Default       	|
|----------------------------	|----------------------------------------------------------------------------------------------------------	|---------------	|
| **AdvanceAHMInterval**     	| Frequency (in seconds) at which vertica checks history retention status                                  	| 180 seconds   	|
| **EpochMapInterval**       	| Determines the granularity of mapping between epochs and time available to historical queries            	| 180 seconds   	|
| **HistoryRetentionTime**   	| Specifies the *amount of time* (in seconds) history is automatically preserved as a historical reference 	| 0 seconds     	|
| **HistoryRetentionEpochs** 	| Specifies the *number of historical epochs* to save, and therefore, the amount of deleted data           	| -1 (disabled) 	|

⚠ *HistoryRetentionTime* and *HistoryRetentionEpochs* are mutually exclusive settings. If both are enabled, *HistoryRetentionTime* takes precedence.