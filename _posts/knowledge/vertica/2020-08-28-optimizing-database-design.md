---
layout: post
title: "Vertica: Optimizing Database Design"
date: 2020-08-28
categories: [knowledge, vertica]
tags: [vertica9.x]
---

# Vertica: Optimizing Database Design

Vertica includes a tool, the **Database Designer (DBD)**, that creates, deploys and optimizes data storage designs. It leverages your typical queries, sample data and schemas, and historical statistics and logs to optimize query performance, data loading, and storage footprint. As a result, we can have faster queries, lower hardware costs, shortened design time, and lower costs to maintain and optimize.

Vertica maintains tables as a logical representation of your data. The data is physically stored on disk in projects, a structure that contains a subset of columns from your table. When data is first loaded, Vertica creates a superprojection - a projection that contains every column in your table in order. While querying from the superprojection will get a faster query response, it is unoptimized - it does not take into account your typical queries or system requirements. After running the DBD, your superprojection will be replaced by one or more optimized superprojections as well as some number of projections that have been built to optimize performance for some queries.

## Database Designer Process

![Database Designer Process Diagram](/assets/images/posts/knowledge/vertica/optimizing-database-design/dbd-process.png)

You need to provide information about your optimization objectives, access to representative data, your most representative queries, current database considerations, etc. The DBD then iteratively considers the queries provided, generates possible projection designs, does a cost/benefit analysis of each prospective query, then chooses the projection(s) that best meet your needs. The next step is for the DBD to test different encoding methods for each column to determine which creates the most efficient storage footprint.

Once the Storage Optimization phase is complete, the DBD outputs:

- **Design Script**: Contains the `CREATE` statement for each projection to be built
- **Deploy Script**: a SQL script that actually creates the projections in the DB.

There are two design types for using the DBD:

- **Comprehensive Design**: Typically done after your first data load, and compares all of your representative queries to your whole database to determine an initial set of projection designs.
- **Incremental Design**: Typically only done after a database has been in use for some time, i.e. if you have a new set of common queries, or the amount/type of information stored has changed.

## DBD Configuration Parameters

DBD Configuration parameters are stored in the `CONFIGURATION_PARAMETERS` System Table. It may be queried like so:

```sql
SELECT parameter_name, default_value, description FROM CONFIGURATION_PARAMETERS WHERE parameter_name ILIKE '%DBD%';
```

⚠ Vertica recommends that you **do not change** these parameters without consultation with professional services.

If you want the DBD to propose replicated/unsegmented projections as part of the design recommendation, these paramerters come into play:

| Parameter Name                  	| Default Value 	| Description                                                                                                                                                            	|
|---------------------------------	|---------------	|------------------------------------------------------------------------------------------------------------------------------------------------------------------------	|
| DBDLargestTableRowCountBoundary 	| 1,000,000     	| Sets the minimum number of rows to define a fact (or "large") table.                                                                                                   	|
| DBDRepSmallRowCountPct          	| 10%           	| If the table has fewer rows than DBDLargestTableRowCountBoundary, check this percentage. If it has fewer rows than this percentage, recommend a replicated projection. 	|
| DBDRepLargeRowCountPct          	| 1%            	| If the table has more rows than DBDLargestTableRowCountBoundary, check this percentage. If it has fewer rows than this percentage, recommend a replicated projection.  	|

![DBD Unsegmented Projections Parameters](/assets/images/posts/knowledge/vertica/optimizing-database-design/dbd-unsegmented-params-1.png)

## Projection Design: Encoding

![Projection Design: Encoding](/assets/images/posts/knowledge/vertica/optimizing-database-design/projection-design-encoding-1.png)

| Parameter Name            	| Default Value 	| Description                                                                                                                            	|
|---------------------------	|---------------	|----------------------------------------------------------------------------------------------------------------------------------------	|
| DBDEncodingSampleRowPct   	| 0             	| **Percentage of rows** to be sampled for encoding analysis during DBD design. <br/> Default setting: Use **DBDEncodingSampleRowCount** 	|
| DBDEncodingSampleRowCount 	| 1,000,000     	| **Number of rows** to be sampled for encoding analysis during DBD design.                                                              	|

## Projection Design: Correlation

The column position in queries can effect the execution performance - if there are columns that contain related data, i.e. the relationship between a state and a zipcode, your query can run faster if the related columns are next to each other.

The DBD can take this relationship into account based on your sample queries by enabling correlation analysis.

| Parameter Name               	| Default Value 	| Description                                                                                                                                  	|
|------------------------------	|---------------	|----------------------------------------------------------------------------------------------------------------------------------------------	|
| DBDCorrelationSampleRowPct   	| 0             	| **Percentage of rows** to be sampled for correlation analysis during DBD design. <br/> Default setting: Use **DBDCorrelationSampleRowCount** 	|
| DBDCorrelationSampleRowCount 	| 1,000,000     	| **Number of rows** to be sampled for correlation analysis during DBD design.                                                                 	|

## Projection Design: Skew