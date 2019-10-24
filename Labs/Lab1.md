# Capturing and Visualizing Metrics and Logs

## Overview

* Setup Elasticsearch
* Setup Kibana
* Deploy Metricbeat to ingest system metrics to Elasticsearch
* Deploy Filebeat to ingest nginx logs, system logs, and elasticsearch logs
* Validate data in Kibana

### Setup Elasticsearch

In this exercise, we will set up Elasticsearch.  The Elasticsearch configuration file is located in `/home/ubuntu/elastic/elasticsearch-7.4.1`.  As you can see we are using the latest Elasticsearch, version 7.4.1

The configuration file is located in the `config` directory.  It is called conveniently, `elasticsearch.yml`.  Please follow these instructions to start your elasticsearch instance.

1. Click on the My lab button on the left if you have not done so.

![My Lab](/Labs/images/virtual_classroom_user_guide_lab-terminal.png)

2. When the terminal comes up, you should be at `/home/ubuntu`
3. `cd elastic/elasticsearch-7.4.1/`
4. To start Elasticsearch, please run `bin/elasticsearch`

### Setup Kibana

We will start Kibana here. The Kibana configuration file is located at `elastic/kibana-7.4.1-linux-x86_64/config`.  You guessed it.  The configuration file is called `kibana.yml`.  Please follow these steps to run Kibana

1. Start a new terminal
2. Go to the Kibana directory `cd elastic/kibana-7.4.1-linux-x86_64/`
3. Start Kibana `bin/kibana`

### Setup Metricbeat

We will start Metricbeat.  By default, the system module is enabled. Namely, the ingest pipeline or collection of metrics and dashboard have been configured for us.

1. Start a new terminal
2. Go to the metricbeat directory, `cd elastic/metricbeat-7.4.1-linux-x86_64/`
3. Start metricbeat, `./metricbeat -e`

### Setup Filebeat

We will set up filebeat, enable the nginx, elasticsearch, and the system modules here.

1. Start a new terminal
2. Go to the filebeat directory, `cd elastic/filebeat-7.4.1-linux-x86_64/`
3. Enable nginx, elasticsearch, and system modules.  Modules provide an easy way to ingest, parse, and graph common log files.

```
./filebeat modules enable nginx
./filebeat modules enable elasticsearch
./filebeat modules enable system
```

4. Let's verify the modules are enabled.

```
./filebeat modules list
```

You should see nginx, elasticsearch, and system in the `enabled` section.
5.  We will have to tell the modules where to find the log files. Typically, there are default locations filebeat will look automatically.  However, for us, the logs are not at their normal location, the `/var/log/` directory except for the system log or syslog.
6. I use nano as my editor `nano modules.d/elasticsearch.yml`
7. In the `elasticsearch.yml` file, please set the `var.paths` under the Server logs section

```
  # Server log:  server:
    enabled: true
    # Set custom paths for the log files. If left empty, 
    # Filebeat will choose the paths depending on your OS.
    var.paths:
     - /home/ubuntu/elastic/elasticsearch-7.4.1/logs/*.log
     - /home/ubuntu/elastic/elasticsearch-7.4.1/logs/*_server.json
```
Please note, you only need to copy and paste the section starting from `var.paths`. Be sure to check the lines are pasted correctly. The yml file is very picky.  Use keyboard control o to save the file and keyboard control x to exit.  
8.  We will do the same for nginx.yml `nano modules.d/nginx.yml`
```
- module: nginx
  # Access logs
  access:
    enabled: true
    # Set custom paths for the log files. If left empty,
    # Filebeat will choose the paths depending on your OS.
    var.paths:
     - /home/ubuntu/data/*.log
```
Please note, you only need to copy and paste the section starting from var.paths. Again, use keyboard control o to save the file and keyboard control x to exit.  
9. Finally, we are ready to start filebeat `./filebeat -e`

### Validate the data in Kibana.

Let's take a look at what we have done so far.

1. Go to Setting on the upper right corner and click on Machine Info.

![Machine Info](/Labs/images/MachineInfo.png)

2. Copy the Public DNS. 

![Public DNS](/Labs/images/RemoteIP.png)

3. Paste the DNS into a browser, add the port number for kibana `:5601` and hit enter.
4. Go to the dashboards.

![dashboards](/Labs/images/dashboards.png)
5. Type in `metricbeat system` in the search bar and hit enter.
6. Select `[Metricbeat System]` ECS Overview
7. Click on Dashboards to go back.
![dashboards1](/Labs/images/dashboards1.png)
8. In the searchbox, type in `filebeat nginx`
9. Select `[Filebeat Nginx] Overview ECS`
10. Be sure to change the date to last 1 year.
![dashboards2](/Labs/images/dashboard2.png)

### Infra and Log UI

Within Kibana, there are two plugins that are dedicated to Infra and Logs.  

1. Click on Infrastructure icon to go to the Infra UI.

![InfraUI](/Labs/images/InfraUI.png)

2. Click on the host to bring up the pop up menu
![infraUI1](/Labs/images/infraUI1.png)

3. Click on View logs to go to the Logs UI or View metrics to go to the Metric UI

This completes Lab1!
