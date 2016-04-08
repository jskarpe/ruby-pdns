## GeoIP Module ##

At present the GeoIP module is very limited, partly by the Ruby GeoIP module and by my personal needs.

### Configuration ###
You need to give the path to your _GeoIP.dat_ file, to do this add a line to the main configuration file:

```
geoip.dblocation = /var/lib/GeoIP/GeoIP.dat
```

The default location if you do not configure it is _/var/lib/GeoIP/GeoIP.dat_

### country ###
Does a simple country code lookup on either _hostname_ or _ip address_.

```
    case country(query[:remoteip])
      when "US", "CA"
        answer.content "64.xx.xx.245"

      when "ZA", "ZW"
        answer.content "196.xx.xx.10"

      else
        answer.content "78.xx.xx.140"
      end
```

On failure - such as missing data file - the lookup will return _nil_ so always be sure to have an _else_ block in your case statements.

**Note**: In order to effectively use this module you must disable caching of packets and responses by PDNS