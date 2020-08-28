---
layout: post
title: "Vertica: Query Execution"
date: 2020-08-28
categories: [knowledge, vertica]
---

# Vertica: Query Execution

## Transactions

Unlike other DBs, once Vertica has written data to files on disk they are never written to again.

## Query Execution

A query submitted to Initiator (any node can be the initiator). Initiator's optimizer chooses Query Plan that has least cost - and then distributes it to other nodes on the cluster (Executors).

Initiator and Executors run the plan locally on the subset of data contained on their node.

Results are sent to the Initiator, which aggregates the results and returns to the submitter.

## Query Plans

Query Plans can be viewed in the console, using the `Query Plan` tab or using the SQL keyword `explain` before a query (this will not execute the query).

Plans should be read from the bottom to the top.

## Timing a Query

Execution time may be viewed in the console or by turning on query timing in the sql client (`\timing` meta-command).