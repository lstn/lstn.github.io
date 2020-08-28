---
layout: post
title: "Vertica: Data Types Cheat Sheet"
date: 2020-08-28
categories: [knowledge, vertica]
tags: [cheatsheet]
---

# Vertica: Data Types Cheatsheet

## 1. Binary data types

| Data Type                	| Size                  	| Description                        	| NULL Sorting 	|
|--------------------------	|-----------------------	|------------------------------------	|--------------	|
| BINARY                   	| 1 to 65,000 bytes     	| Fixed-length binary string         	| NULLS LAST   	|
| VARBINARY, BYTEA, or RAW 	| 1 to 65,000 bytes     	| Variable-length binary string      	| NULLS LAST   	|
| LONG VARBINARY           	| 1 to 32,000,000 bytes 	| Long variable-length binary string 	| NULLS LAST   	|

## 2. Boolean data types

| Data Type 	| Size   	| Description           	| NULL Sorting 	|
|-----------	|--------	|-----------------------	|--------------	|
| BOOLEAN   	| 1 byte 	| True or False or NULL 	| NULLS LAST   	|

## 3. Character data types

| Data Type    	| Size            	| Description                           	| NULL Sorting 	|
|--------------	|-----------------	|---------------------------------------	|--------------	|
| CHAR         	| 1 to 65,000     	| Fixed-length character string         	| NULLS LAST   	|
| VARCHAR      	| 1 to 65,000     	| Variable-length character string      	| NULLS LAST   	|
| LONG VARCHAR 	| 1 to 32,000,000 	| Long variable-length character string 	| NULLS LAST   	|

## 4. Date and Time data types

| Data Type                             	| Size             | Description                              	    | NULL Sorting 	|
|---------------------------------------	|---------  |------------------------------------------------       |--------------	|
| DATE                                  	| 8 bytes   | A month, day, and year                   	            | NULLS FIRST  	|
| TIME                                  	| 8 bytes   | A time of day without timezone           	            | NULLS FIRST  	|
| TIME WITH TIMEZONE                    	| 8 bytes   | A time of day with timezone              	            | NULLS FIRST  	|
| TIMESTAMP, DATETIME, or SMALLDATETIME 	| 8 bytes   | A date and time without timezone         	            | NULLS FIRST 	|
| TIMESTAMP WITH TIMEZONE               	| 8 bytes   | A date and time with timezone           	            | NULLS FIRST 	|
| INTERVAL                              	| 8 bytes   | Measures the difference between two points in time    | NULLS FIRST 	|
| INTERVAL DAY TO SECOND                	| 8 bytes   | An interval measured in days and seconds 	            | NULLS FIRST  	|
| INTERVAL YEAR TO MONTH                	| 8 bytes   | An interval measured in years and months 	            | NULLS FIRST  	|

## 5. Approximate Numeric data types

| Data Type        	| Size    	| Description                                                            	| NULL Sorting 	|
|------------------	|---------	|------------------------------------------------------------------------	|--------------	|
| DOUBLE PRECISION 	| 8 bytes 	| Signed 64-bit IEEE floating point number, requiring 8 bytes of storage 	| NULLS LAST   	|
| FLOAT            	| 8 bytes 	| Signed 64-bit IEEE floating point number, requiring 8 bytes of storage 	| NULLS LAST   	|
| FLOAT(n)         	| 8 bytes 	| Signed 64-bit IEEE floating point number, requiring 8 bytes of storage 	| NULLS LAST   	|
| FLOAT8           	| 8 bytes 	| Signed 64-bit IEEE floating point number, requiring 8 bytes of storage 	| NULLS LAST   	|
| REAL             	| 8 bytes 	| Signed 64-bit IEEE floating point number, requiring 8 bytes of storage 	| NULLS LAST   	|

## 6. Exact Numeric data types

| Data Type 	| Size     	| Description                                                                              	| NULL Sorting 	|
|-----------	|----------	|------------------------------------------------------------------------------------------	|--------------	|
| INTEGER   	| 8 bytes  	| Signed 64-bit integer, requiring 8 bytes of storage                                      	| NULLS FIRST  	|
| INT       	| 8 bytes  	| Signed 64-bit integer, requiring 8 bytes of storage                                      	| NULLS FIRST  	|
| BIGINT    	| 8 bytes  	| Signed 64-bit integer, requiring 8 bytes of storage                                      	| NULLS FIRST  	|
| INT8      	| 8 bytes  	| Signed 64-bit integer, requiring 8 bytes of storage                                      	| NULLS FIRST  	|
| SMALLINT  	| 8 bytes  	| Signed 64-bit integer, requiring 8 bytes of storage                                      	| NULLS FIRST  	|
| TINYINT   	| 8 bytes  	| Signed 64-bit integer, requiring 8 bytes of storage                                      	| NULLS FIRST  	|
| DECIMAL   	| 8+ bytes 	| 8 bytes for the first 18 digits of precision, plus 8 bytes for each additional 19 digits 	| NULLS FIRST  	|
| NUMERIC   	| 8+ bytes 	| 8 bytes for the first 18 digits of precision, plus 8 bytes for each additional 19 digits 	| NULLS FIRST  	|
| NUMBER    	| 8+ bytes 	| 8 bytes for the first 18 digits of precision, plus 8 bytes for each additional 19 digits 	| NULLS FIRST  	|
| MONEY     	| 8+ bytes 	| 8 bytes for the first 18 digits of precision, plus 8 bytes for each additional 19 digits 	| NULLS FIRST  	|

## 7. Spatial data types

| Data Type 	| Size                  	| Description                                                                     	| NULL Sorting 	|
|-----------	|-----------------------	|---------------------------------------------------------------------------------	|--------------	|
| GEOMETRY  	| 1 to 10,000,000 bytes 	| Coordinates expressed as (x,y) pairs, defined in the Cartesian plane.           	| NULLS LAST   	|
| GEOGRAPHY 	| 1 to 10,000,000 bytes 	| Coordinates expressed in longitude/latitude angular values, measured in degrees 	| NULLS LAST   	|

## 8. UUID data types

| Data Type 	| Size     	| Description                                    	| NULL Sorting 	|
|-----------	|----------	|------------------------------------------------	|--------------	|
| UUID      	| 16 bytes 	| Stores universally unique identifiers (UUIDs). 	| NULLS LAST   	|
