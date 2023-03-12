Contains HM Land Registry data © Crown copyright and database right 2021.
This data is licensed under the Open Government Licence v3.0.

See https://clickhouse.com/docs/en/getting-started/example-datasets/uk-price-paid/

## Demo

```
# Download ClickHouse:
$ curl https://clickhouse.com/ | sh

$ ./clickhouse local 
ClickHouse local version 23.3.1.2537.

:) CREATE DATABASE test;

Ok.

:) USE test;

Ok.

:) ATTACH TABLE uk_price_paid UUID 'cf712b4f-2ca8-435c-ac23-c4393efe52f7'
(
    price UInt32,
    date Date,
    postcode1 LowCardinality(String),
    postcode2 LowCardinality(String),
    type Enum8('other' = 0, 'terraced' = 1, 'semi-detached' = 2, 'detached' = 3, 'flat' = 4),
    is_new UInt8,
    duration Enum8('unknown' = 0, 'freehold' = 1, 'leasehold' = 2),
    addr1 String,
    addr2 String,
    street LowCardinality(String),
    locality LowCardinality(String),
    town LowCardinality(String),
    district LowCardinality(String),
    county LowCardinality(String)
)
ENGINE = MergeTree
ORDER BY (postcode1, postcode2, addr1, addr2)
SETTINGS disk = disk(type = web, endpoint = 'https://raw.githubusercontent.com/ClickHouse/web-tables-demo/main/web/')

Query id: 21982815-9290-44e6-bc99-595ab0080411

Ok.

0 rows in set. Elapsed: 1.691 sec. 

:) SELECT
    toYear(date) AS year,
    round(avg(price)) AS price,
    bar(price, 0, 1000000, 80)
FROM uk_price_paid
GROUP BY year
ORDER BY year ASC

Query id: 7aa7cba7-eac5-40a8-9e0c-326eeb3b7de6

┌─year─┬──price─┬─bar(round(avg(price)), 0, 1000000, 80)─┐
│ 1995 │  67935 │ █████▍                                 │
│ 1996 │  71510 │ █████▋                                 │
│ 1997 │  78538 │ ██████▎                                │
│ 1998 │  85442 │ ██████▊                                │
│ 1999 │  96038 │ ███████▋                               │
│ 2000 │ 107489 │ ████████▌                              │
│ 2001 │ 118891 │ █████████▌                             │
│ 2002 │ 137955 │ ███████████                            │
│ 2003 │ 155895 │ ████████████▍                          │
│ 2004 │ 178890 │ ██████████████▎                        │
│ 2005 │ 189361 │ ███████████████▏                       │
│ 2006 │ 203532 │ ████████████████▎                      │
│ 2007 │ 219376 │ █████████████████▌                     │
│ 2008 │ 217043 │ █████████████████▎                     │
│ 2009 │ 213421 │ █████████████████                      │
│ 2010 │ 236113 │ ██████████████████▉                    │
│ 2011 │ 232807 │ ██████████████████▌                    │
│ 2012 │ 238384 │ ███████████████████                    │
│ 2013 │ 256928 │ ████████████████████▌                  │
│ 2014 │ 280029 │ ██████████████████████▍                │
│ 2015 │ 297282 │ ███████████████████████▊               │
│ 2016 │ 313543 │ █████████████████████████              │
│ 2017 │ 346486 │ ███████████████████████████▋           │
│ 2018 │ 350913 │ ████████████████████████████           │
│ 2019 │ 352562 │ ████████████████████████████▏          │
│ 2020 │ 376855 │ ██████████████████████████████▏        │
│ 2021 │ 382166 │ ██████████████████████████████▌        │
│ 2022 │ 387415 │ ██████████████████████████████▉        │
└──────┴────────┴────────────────────────────────────────┘

28 rows in set. Elapsed: 0.431 sec. Processed 27.21 million rows, 163.28 MB (63.17 million rows/s., 379.00 MB/s.)

:) Bye.
```
