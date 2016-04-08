## Time Based Responses ##

Lets say you have multiple Datacenters and want to give your highest paying customers a URL to log in, maybe your login forms generate the url, you can do this based on Time like in this example.

```
module Pdns
  newrecord("vip.your.net") do |query, answer|
      hr = Time.now.hour

      case hr
         when 0..9
            content.answer "1.x.x.x"
         when 10..20
            content.answer "2.x.x.x"
         when 21..23
            content.answer "3.x.x.x"
      end
  end
end
```

This example will use the clock on your server and make decisions based on the time there, perhaps not as useful as you'd hope but a good enough description of the approach to take.