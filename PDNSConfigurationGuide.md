# Power DNS Configuration Guide #

This is a guide about some of the PowerDNS configuration options and how they affect us, it is _not_ an exhaustive guide to PowerDNS please see their official documentation at http://doc.powerdns.com/

These options generally goes into _/etc/pdns/pdns.conf_ though where exactly depends on your distribution and build.

For settings affecting this backend directly like log locations and such please see [ReferenceConfigurationGuide](ReferenceConfigurationGuide.md).

## Launching the backend ##
These settings configure where to find the runner, how many instances to launch and what version of the protocol to talk to it.

We need abi-version 2 so be sure to set that else it will not work.  If you are using just the pipe backend the example below is fine, else see the PDNS docs about chaining backends.

Usually PDNS will launch and kill threads to cope with demand, you may want to make it start more from the onset using _distributor-threads_

```
launch=pipe
pipe-command=/usr/bin/pdns-pipe-runner.rb
pipebackend-abi-version=2
distributor-threads=3
```

## Restricting what queries to send to the backend ##
PowerDNS will send a lot of junk to the backend, this generally is fine as most of it will just get ignored but if you find it a problem you can give PowerDNS some regex to tell it what to send to the Pipe:

```
pipe-regex=^foo.your.net;(A|ANY|SOA)$
```

In the example above we will get just _A_, _ANY_ and _SOA_ records for _foo.your.net_.  You need to be careful with this the setting above is probably the minimum, if you do not send SOA requests to the backend you will get weird results and _ServFail_ messages on DNS clients.

A more complex example would be, it clear that u need to test this quite carefully and think it through before enabling it.

```
pipe-regex=^(foo.your.net;(A|ANY|SOA))|(other.your.net;(A|ANY|SOA))$
```

## Disabling Shuffling ##
If you want to use the randomization functions to carefully control the order of records that get returned you must disable this in PowerDNS else it will shuffle the responses before sending it to the client.

```
no-shuffle=yes
```

## Disable fancy CNAME processing ##
PowerDNS feels it needs to recurse CNAMEs and try to resolve them locally, this might not be desired and it is different from how other nameservers work, I tend to disable this behaviour:

```
skip-cname=yes
```

If however you host for example _a.your.com_ and _b.your.com_ on your ruby-pdns, with _a_ being a CNAME for _b_ then you _must_ set this option to _no_ else it won't work.

## Disabling processing out-of-zone data ##
If you use PowerDNS to just host records using this backend then you should disable this, else leave it on else your other records and domains might break, this option and the CNAME processing above should go hand-in-hand

```
out-of-zone-additional-processing=no
```

## PowerDNS internal caches ##
PowerDNS is really fact because it does a lot of caching.  Caching though will cause havok if you have GeoIP based records or one that do special processing on every remote address.  If needed you can disable all the caches like this:

```
query-cache-ttl=0
negquery-cache-ttl=0
cache-ttl=0
```

Be careful though turning this off means every single query will hit your code and might impact performance badly.