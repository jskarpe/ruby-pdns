## Information about Requests ##
Each record gets passed a [Hash](http://ruby-doc.org/core/classes/Hash.html) containing information about the request, reviewing how a new record gets created:

```
module Pdns
  newrecord("www.your.net") do |query, answer|
     answer.qtype = query[:qtype]
    
     # rest removed
  end
end
```

The _query_ in this case would be a hash of the request, below is a dump of such a request:

```
{:qname       => "www.your.net",
 :qclass      => :IN,
 :qtype       => :ANY,
 :id          => 1,
 :remoteip    => "192.168.1.1",
 :localip     => "0.0.0.0"}
```

The contents of the hash are:

|:qname|The exact request that was received, this could be mixEdcase text so if you need to do matching on this keep that in mind, responses should preserve the case of the request.|
|:-----|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|:qclass|At present the only supported value is :IN                                                                                                                                   |
|:qtype|This can be values like :SRV, :ANY, :SOA, :TXT etc, any standard DNS record type that [PDNS supports](http://doc.powerdns.com/types.html)                                    |
|:id   |PDNS assigns an ID to responses and later can ask you for a AXFR on just the ID, this value is currently unused by this framework but it's in the TODO to support it, it's passed on for completeness sake only at this point|
|:remoteip|This is the IP address of the requester, if you are using Bind as a frontend proxying requests to PDNS then this would be the IP of the Bind server                          |
|:localip|This is the IP the PDNS server received the request on, only useful really on multiple homed machines that are not set to listen on _0.0.0.0_                                |

You access the hash values like you would any hash in Ruby, see line 3 of the example above.