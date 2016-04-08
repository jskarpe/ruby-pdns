## Geographical Location based Responses ##

This is probably the most often desired use case, you have a website and some static content, you have static mirrors in several places and want to serve it to your client from their nearest mirror.

<!> If you want to do GeoIP based lookups your clients must hit the PDNS server directly, you cannot have them proxied to the PDNS server via bind.

<!> You should also when using GeoIP based lookups disable all caching in PDNS, else PDNS will only run your logic when the cache invalidates.

RPDNS includes at present just a country lookup method using maxmind geoip, in future versions we'll support city, long/lat and so forth.

```
module Pdns
  newrecord("static.your.net") do |query, answer|
    case country(query[:remoteip])
      when "US", "CA"
        answer.content "64.xx.xx.245"

      when "ZA", "ZW"
        answer.content "196.xx.xx.10"

      else
        answer.content "78.xx.xx.140"
      end
  end
end
```

The simple case uses the query hash to access the remote IP address - the IP address of the requester - and uses that to make a decision based on country.

Defaults for TTL and Type are being used - TTL of 3600 A record.