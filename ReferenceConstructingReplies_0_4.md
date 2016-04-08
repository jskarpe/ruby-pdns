﻿#summary Constructing the Answer
#labels Phase-Historical,Deprecated

## Constructing the Answer ##



Each record gets passed a Pdns::Response object containing information about the answer(s), reviewing how a new record gets created:

```
module Pdns
  newrecord("www.your.net") do |query, answer|
     answer.qtype = query[:qtype]
    
     # rest removed
  end
end
```

As you see you have an **answer** object, this is your only avenue for affecting the response to the PDNS server.

  * You can set a TTL for all records returned or per record
  * You can set the Type for all records return of per record
  * You can respond with one of the following record types only :A, :CNAME, :SOA, :NS, :SRV, :MX, :TXT, more can be added in future
  * You can respond with multiple records like multiple MX, NS or A records for round robins etc
  * The server can shuffle your responses randomly or you can disable this feature

### Setting the TTL ###
You can set the TTL for all response records that do not have a specific TTL specified:

```
answer.ttl 1800
```

|Defaults to:|3600|
|:-----------|:---|
|Valid Values:|Integer Numbers|
|Exception Raised:|Pdns::InvalidTTL|

[[Anchor(qtype)]]
### Setting the type of response ###
The type gets set using Ruby Symbols, some are simple some have some catches, see the table below, see the examples later on under the section for setting the content.

```
answer.qtype :A
```

|Defaults to:|:A|
|:-----------|:-|
|Valid Values:|:A, :CNAME, :NS, :SRV, :MX, :TXT|
|Exception Raised:|Pdns::UnknownQueryType|

### Setting the Query Class ###
At present only _IN_ records can be returned so setting this will have little effect, it's for future use only

```
answer.qclass :IN
```

|Defaults to:|:IN|
|:-----------|:--|
|Valid Values:|:IN|
|Exception Raised:|Pdns::UnknownQueryClass|

### Disabling response shuffling ###
By default if you return more than one record they will be shuffled randomly, you can disable this if you wish to affect the order of returned records.

```
answer.shuffle false
```


|Defaults to:|true|
|:-----------|:---|
|Valid Values:|true, false|
|Exception Raised:|Pdns::InvalidShuffle|

  * ote:**For this to have any effect you need to also set _no-shuffle=yes_ in the PDNS configuration.**

### Returning Records ###
#### Returning Simple Records ####
You can return many simple records by calling this many times

```
answer.ttl 1800
answer.qtype :A
answer.content "192.168.1.2"
answer.content "192.168.1.1
```

In the example above you'll be returning a Round Robin record with 2 records, each will have a TTL of 1800 and A records.

|Defaults to:|n/a|
|:-----------|:--|
|Valid Values:|Dependant on qtype|
|Exception Raised:|n/a|

#### Setting Custom TTL and Type per record ####
In some cases you'll want to set specific TTLs and Types for each record, this is possible using the same _content_ method:

```
answer.content :MX, "10       mx1.foo.com"
answer.content 3600, :TXT, "v=spf1 include:another"
answer.ttl 1800
```

The example above will set a default TTL of 1800, the first MX record will have a TTL of 1800 and the TXT record will have a TTL of 3600.  You can use any combination of these techniques or mix them all you want.

Note this example sets the default after the records, this is valid the default will still be 1800 and be applied when not otherwise specified.

#### Creating specific types of responses ####
PowerDNS is a bit tricky at times, you need to insert tabs into the response in the right place, in the samples below a "\t" string indicates a tab.

|**Record Type**|**Sample**|
|:--------------|:---------|
|:A             |answer.content :A, "127.0.0.1"|
|:TXT           |answer.content :TXT, "This is some text"|
|:MX            |answer.content :MX, "10\tmx.foo.com"|
|:SRV           |answer.content :SRV, "1 0 1719 sipgw1"|
|:CNAME         |answer.content :CNAME, "www.foo.com"|
|:NS            |answer.content :NS, "ns1.foo.com"|