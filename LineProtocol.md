# Line Protocol
The line protocol is a text based format for writing points to InfluxDB. Each line defines a single point. Multiple lines must be separated by the newline character \n. The format of the line consists of three parts:
``` javascript
[key] [fields] [timestamp]
```
Each section is separated by spaces. The minimum required point consists of a measurement name and at least one field. Points without a specified timestamp will be written using the server’s local timestamp. Timestamps are assumed to be in nanoseconds unless a precision value is passed in the query string.

# Writing and exploring data
Now that we have a database, InfluxDB is ready to accept queries and writes.

First, a short primer on the datastore. Data in InfluxDB is organized by “time series”, which contain a measured value, like “cpu_load” or “temperature”. Time series have zero to many points, one for each discrete sample of the metric. Points consist of time (a timestamp), a measurement (“cpu_load”, for example), at least one key-value field (the measured value itself, e.g. “value=0.64”, or “temperature=21.2”), and zero to many key-value tags containing any metadata about the value (e.g. “host=server01”, “region=EMEA”, “dc=Frankfurt”).

Conceptually you can think of a measurement as an SQL table, where the primary index is always time. tags and fields are effectively columns in the table. tags are indexed, and fields are not. The difference is that, with InfluxDB, you can have millions of measurements, you don’t have to define schemas up-front, and null values aren’t stored.

Points are written to InfluxDB using the Line Protocol, which follows the following format:
``` javascript
<measurement>[,<tag-key>=<tag-value>...] <field-key>=<field-value>[,<field2-key>=<field2-value>...] [unix-nano-timestamp]
```
The following lines are all examples of points that can be written to InfluxDB:
``` javascript
cpu,host=serverA,region=us_west value=0.64
payment,device=mobile,product=Notepad,method=credit billed=33,licenses=3i 1434067467100293230
stock,symbol=AAPL bid=127.46,ask=127.48
temperature,machine=unit42,type=assembly external=25,internal=37 1434067467000000000
```
