# Rough Draft #

Doing RPM test:
  * build a rc RPM, need to comment out the gem stuff cos they want pure numeric
  * rm -rf /etc/pdns/pdns-ruby-backend.cfg`*` /etc/pdns/records /var/log/pdns
  * rpm -ivh ruby-pdns-0.4-rc1.el5.noarch.rpm
  * configure as per wiki docs
  * copy test suite onto the machine - test suite only not the whole app
  * run test suite against installed copy of ruby-pdns
  * copy sample test records onto server and test with dig from remote machine

Doing Release Candidate:
  * Create branch
  * set ENV variables to make rc, rake rpm  after commenting out the gem tasks
  * tar the release
  * upload tar, rpm and srpm to google code
  * copy the reference docs to release ones if major release
  * update changelog