# Overview #

Statistics is important, if you're going to have the ability to influence the records using external data, weights and country data on a per record basis you need a way to monitor it.

A statistics system has been developed in a way that will easily plug into Cacti as well as most other systems as it returns simple key:value pairs of stats.

Each record has statistics for how many times it was queried but also how much time was spent serving that request.  A combined total is also kept.

Several components are involved in producing the stats, these are:

  * Each worker periodically writes a stat file with it's process ID in the file name
  * A cron job reads the stats, discards old stats and produce an aggregate file containing the combined stats for all workers
  * A script - _pdns-get-stats_ - outputs statistics for a specified record in the format prescribed by the Cacti plugin system

**NOTE:** This feature is available from release 0.5 onward.

## Configuration ##
Recording stats is enabled by default, the below two configuration options affect where and if they will get logged.

|**Option**|**Sample**|**Possible Values**|**Description**|
|:---------|:---------|:------------------|:--------------|
|statsdir  |/var/log/pdns/stats|Any directory writable _pdns_ user|Directory where stats will be written|
|recordstats|true      |true, false        |Set to true to keep stats, else false|
|maint\_interval|60        |Integer numbers in seconds|How often to run maintenance like dumping stats and clearing caches|

A cronjob will be saved - usually in _/etc/cron.d/rubypdns_ - it runs every 5 minutes and creates the aggregate data.

If you installed using the Gem you need to add this cronjob yourself, place the following in _/etc/cron.d/rubypdns_:

```
*/5 * * * *  pdns /usr/sbin/pdns-aggregate-stats.rb --config /etc/pdns/pdns-ruby-backend.cfg
```

As long as this cronjob runs no old data will be left in the stats dir, it will clean up files slightly older than the _maint\_interval_ setting in the configuration file.

## Extracting Statistics ##
A script is provided to extract stats from the aggregate data, you can simply run it as below:

```
# /usr/sbin/pdns-get-stats.rb --record ruby-pdns-totals
usagecount:30 totaltime:0.00759959220886231 averagetime:0.0002533
# /usr/sbin/pdns-get-stats.rb --usec --record ruby-pdns-totals
usagecount:30 totaltime:7599 averagetime:253
# /usr/sbin/pdns-get-stats.rb --record your.domain.com
usagecount:10 totaltime:0.00123 averagetime:0.00123
```

You can use this script via [NRPE](http://nagios.sourceforge.net/docs/3_0/addons.html) or SNMP for inclusion in any stats system you choose.  For a sample of using NRPE from Cacti see the [Cacti Graphs we provide for PDNS](PDNSCactiGraphs.md).

## Future Plans ##
In future we intend to write scripts like the _pdns-get-stats.rb_ script that can run periodically to update RRD file or send data to collectd etc, this will be based on user demand.

We will also provide Cacti Templates that can be used to graph this data.