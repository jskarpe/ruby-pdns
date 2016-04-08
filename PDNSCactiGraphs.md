# Graphing PDNS using Cacti #
There are several ways to monitor PowerDNS through Cacti, the data template we provide makes use of [NRPE](http://nagios.sourceforge.net/docs/nrpe/NRPE.pdf) (Nagios Remote Plugin Executor).

Therefore the following steps will just apply to the above method but with a bit of fiddling you can use it with other input methods such SNMP.

## Configuring PowerDNS Server ##

  * Place **cacti\_pdns.rb** under your **/usr/local/bin** path
  * If you run your nrpe plugins under an unprivileged user (e.g nagios) then a sudoers entry needs to be added:
```
nagios ALL = NOPASSWD: /usr/local/bin/cacti_pdns.rb
```
  * Create a configuration file for the nrpe plugin and set up the check as follows:
```
command[check_cacti-pdns]=sudo /usr/local/bin/cacti_pdns.rb
```

## Configuring Cacti Server ##

  * Import the templates into Cacti through the Console management under Import/Export templates. In the example shown below the data template has been imported:

> ![http://www.devco.net/images/cacti/cacti_import_templates.png](http://www.devco.net/images/cacti/cacti_import_templates.png)

  * Once all the templates have been imported into Cacti you will have to set up the Data Sources for the hosts on which you have PowerDNS running in order to fetch the data from them.  Therefore you need to go to Management -> Data Sources and then click on Add which can be found in the top right corner of the page, as the screenshot underneath shows.

> ![http://www.devco.net/images/cacti/cacti_datasource_add.png](http://www.devco.net/images/cacti/cacti_datasource_add.png)

> The next interface is very straightforward. You just need to specify as a data template 'PowerDNS stats' the PowerDNS host you will be monitoring.

> ![http://www.devco.net/images/cacti/cacti_select_datasource.png](http://www.devco.net/images/cacti/cacti_select_datasource.png)

> After you provided the required information click on Create and you will get to a page like this where you just have to save the newly-created data source.

> ![http://www.devco.net/images/cacti/cacti_save_datasource.png](http://www.devco.net/images/cacti/cacti_save_datasource.png)

  * Last step will be creating the graphs for the PowerDNS hosts. Still under Management, go to Graph Management and then click on Add at the top right corner of the page.

> ![http://www.devco.net/images/cacti/cacti_graph_mgmt_add_graph.png](http://www.devco.net/images/cacti/cacti_graph_mgmt_add_graph.png)

> In the next page you will need to specify which graph template you want to use to create the new graph and the PowerDNS host you will be monitoring

> ![http://www.devco.net/images/cacti/cacti_select_graph_template.png](http://www.devco.net/images/cacti/cacti_select_graph_template.png)

> Once selected click on Create and in the next and last interface you will be required to select which data source will be used to graph each item.

> ![http://www.devco.net/images/cacti/cacti_select_graph_datasource.png](http://www.devco.net/images/cacti/cacti_select_graph_datasource.png)

> At this point the graph is ready, click on Save and you will see that the graph has been added to the PowerDNS host.
> As data needs collecting it might take a while for the graphs to show up with some relevant information.

> Repeat the process to add the rest of the graphs.