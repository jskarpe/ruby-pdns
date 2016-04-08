# Testing Records #

While writing records you'd want to test them, typically you'd have a dev server and drop them into a running instance of the code.  That's not always ideal so a little tester script was written.

**Note:** This feature is only available from 0.4 onward.

## Setup ##
You need to provide the tester a config file, you'd want to log elsewhere, run in debug mode and point to a different set of records, here is a sample config file:

```
logfile = /dev/stderr
loglevel = debug
records_dir = records
soa_contact = dns-admin.pinetecltd.net
soa_nameserver = ns1.pinetecltd.net
reload_interval = 30
keep_logs = 10
max_log_size = 1024000
geoipdb = /var/lib/GeoIP/GeoIP.dat
```

It is exactly the same as the running config except we've changed _logfile_, _loglevel_ and _records\_dir_ so that it will log to your console in debug mode and read records from your current _./records_.

Now drop your new test record in there and run the tester script:

```
$ ls records/your.test.com.prb
records/your.test.com.prb

$ pdns-pipe-tester.rb --config etc/pdns-ruby-backend-debug.cfg --record your.test.com
W, [2009-08-11T18:37:50.747130 #9423]  WARN -- : 9423 ./pdns/runner.rb:35:in `initialize': Tester starting
W, [2009-08-11T18:37:50.747871 #9423]  WARN -- : 9423 ./pdns/runner.rb:48:in `load_records': Loading new record from records/your.test.com.prb
D, [2009-08-11T18:37:50.748064 #9423] DEBUG -- : 9423 ./pdns/resolvers.rb:23:in `add_resolver': Adding resolver your.test.com into list of workers

Response for ANY query on your.test.com from 127.0.0.1

Response for your.test.com:
          Default TTL: 600
         Default Type: A
        Default Class: IN
                   ID: 1
              Shuffle: true

        Records:
                       your.test.com    IN      A       600     1       64.x.x.244
```

This will do an _ANY_ query against _your.test.com_ you can pass _--type A_ to do specific types of query.

To test your GeoIP based records you can run with _--remoteip=1.2.3.4_ and the record will be ran as if the remote ip were doing the call.