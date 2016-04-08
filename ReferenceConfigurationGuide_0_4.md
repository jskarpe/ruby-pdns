﻿#summary Ruby PDNS configuration guide
#labels Phase-Historical,Deprecated

# Ruby PDNS configuration guide #
Basic configuration goes into _/etc/pdns/pdns-ruby-backend.cfg_ by default, you can though specify an alternative location using the _--config_ command line option.

The config file is a simple INI file format of just _key=val_ pairs, the keys and possible values are listed below.

Comments starting with _#_ and new lines are allowed and will be ignored.

|**Option**|**Sample**|**Possible Values**|**Description**|
|:---------|:---------|:------------------|:|
|logfile   |/var/log/pdns/pipe-backend.log|Any file           |Where to write log messages to, for debugging you can use /dev/stderr even|
|loglevel  |The level to log at|info|error|warn|fatal|debug|Warn is generally a good level to use for production|
|records\_dir|/etc/pdns/records|Any directory      |Where to find **_.prb_ files**|
|reload\_interval|60        |Integer numbers    |How often in seconds to reload records from _records`_`dir_|
|soa\_contact|postmaster.your.net|Email address as acceptable in SOA records|We create fake SOA records, this will be the contact|
|soa\_nameserver|ns1.your.net|The primary nameserver listed in SOA records|We create fake SOA records, this will be the nameserver|
|keep\_logs|10        |Integer number     |How many logs to keep|
|max\_log\_size|1024000   |Integer number     |What size log to keep|
|maint\_interval|60        |Integer numbers in seconds|How often to run maintenance like dumping stats and clearing caches|

Additional module configuration can be done in this file using lines like these:

```
geoip.dblocation = /var/lib/GeoIP/GeoIP.dat
```

The meaning of these will be documented in the docs for each specific module like the [GeoIP module](ReferenceGeoIP.md)

A sample file can be seen below

```
# Ruby PowerDNS Config File
logfile = /var/log/pdns/pipe-backend.log
loglevel = info
records_dir = /etc/pdns/records
soa_contact = dns-admin.foo.net
soa_nameserver = ns1.foo.net
reload_interval = 60
keep_logs = 10
max_log_size = 1024000
maint_interval = 60
geoip.dblocation = /var/lib/GeoIP/GeoIP.dat
```

There are many settings in PowerDNS that can affect the performance and effectiveness of this backend, please see [PDNSConfigurationGuide](PDNSConfigurationGuide.md) for details on those