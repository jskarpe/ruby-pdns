﻿#summary Release Notes for 0.4
#labels Phase-Historical,Deprecated

# Introduction #
We're pleased to announce release 0.4 of Ruby PDNS.

Release 0.3 was a feature complete release but lacked full exception handling, the 0.4 branch is primarily concerned with stability and as such has a lot of new exception handling, unit test coverage and some code refactoring to make future releases easier.

There should now be very few causes for the backend just not to start or just randomly exit without logging useful information to the log file.  The only case we're aware of is when the logfile cannot be written to which is treated as a critical failure.

In addition to robustness improvements we've now added a tool that can be used to test your records outside of your nameserver, this should help with syntax and logic checking even with GeoIP as you can specify custom source addresses etc.  See [TestingRecords](TestingRecords.md)

This release is available as a tarball, RPM, Source RPM and RubyGem on the [downloads](http://code.google.com/p/ruby-pdns/downloads/list?can=2&q=0.4) page.

This release does not break any backward compatibility with your records, however you do need to make a small change to the configuration file.  Please see further down below.

## Documentation ##
We tag the documentation for each release so you always have a good point in time reference, please find below links to Reference documentation for this branch

  * [Install Guide](ReferenceInstallGuide_0_4.md)
  * [Configuration Reference](ReferenceConfigurationGuide_0_4.md)
  * [Constructing Replies](ReferenceConstructingReplies_0_4.md)
  * [About Requests](ReferenceInfoAboutRequests_0_4.md)
  * [GeoIP](ReferenceGeoIP_0_4.md)
  * [Ruby Extensions](ReferenceRubyExtensions_0_4.md)

## Upgrading ##
We now provide a RubyGem for operating systems other than RedHat, it's a new release target and will be the preferred method for non RedHat systems till we can get a volunteer to build proper packages for other distros.

### Source Installs ###
To upgrade using the RubyGem if you installed by hand in the past please do:

  * Make note of your Ruby Site Lib Directory: ruby -rrbconfig -e "puts Config::CONFIG'sitelibdir'"
  * Delete _pdns.rb_ from your Ruby Sitelib Directory
  * Delete the _pdns_ directory from your Ruby Sitelib Directory
  * Delete _/usr/sbin/pdns-pipe-runner.rb_

Now follow the install guide as if you're a new install of RubyGems

### RPM Installs ###
The RPM should cleanly upgrade using just _rpm -Uvh ruby-pdns-0.4.el5.noarch.rpm_

### Configuration ###
This release requires you to make changes to your config file, for a full reference of the config file please see [Configuration Reference](ReferenceConfigurationGuide_0_4.md), the changes in this release are:

  * The old _geoipdb_ option has been removed, should now be _geoip.dblocation_
  * A new option has been added - _maint\_interval_ - this defaults to 60

## Changelog ##
  * Minor maintainability updates [#13](http://code.google.com/p/ruby-pdns/issues/detail?id=13), [#18](http://code.google.com/p/ruby-pdns/issues/detail?id=18)
  * Unparsable config lines should not prevent the framework from starting [#16](http://code.google.com/p/ruby-pdns/issues/detail?id=16)
  * Records that dont compile or raise exceptions should not cause the framework to die [#14](http://code.google.com/p/ruby-pdns/issues/detail?id=14)
  * New script has been added to facilitate testing of records interactively [#15](http://code.google.com/p/ruby-pdns/issues/detail?id=15)
  * Start putting in place a system to do periodic maintenance and stats tasks [#2](http://code.google.com/p/ruby-pdns/issues/detail?id=2)
  * Add cacti monitoring templates to monitor a generic PDNS install [#17](http://code.google.com/p/ruby-pdns/issues/detail?id=17)
  * The GeoIP code ignored the configured path to the geoip data and so blew up pretty badly, code is now more robust in handling errors and uses the config.  Thanks Christian Keil for the bug report.  [#20](http://code.google.com/p/ruby-pdns/issues/detail?id=20)
  * Added a rubygem build task and updated install docs etc [#19](http://code.google.com/p/ruby-pdns/issues/detail?id=19)
  * Change the logging output to only show the filename not the full path of the file that produced the log entry
  * Now prints the correct file and line where log lines are being created from  [#22](http://code.google.com/p/ruby-pdns/issues/detail?id=22)
  * Modules like GeoIP can now have seperate config stanzas  [#7](http://code.google.com/p/ruby-pdns/issues/detail?id=7)
  * GeoIP now has it's own config bits in config file  [#23](http://code.google.com/p/ruby-pdns/issues/detail?id=23)
  * Initial set of unit tests have been added using ruby Test::Unit  [#24](http://code.google.com/p/ruby-pdns/issues/detail?id=24)