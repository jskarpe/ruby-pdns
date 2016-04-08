## GeoIP Module ##

At present the GeoIP module is very limited, partly by the Ruby GeoIP module and by my personal needs.

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


**Note**: In order to effectively use this module you must disable caching of packets and responses by PDNS