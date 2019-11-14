# Capturing and Visualizing Metrics and Logs

## Overview

* Deploy Metricbeat to ingest system metrics to Elasticsearch
* Deploy Filebeat to ingest nginx logs, system logs, and elasticsearch logs
* Validate data in Kibana

### Setup Metricbeat

We will be working in Strigo, a virtual classroom. First, we will start Metricbeat. By default, the system module is enabled. Namely, the ingest pipeline or collection of metrics and dashboard have been configured for us.

1. Click on the My lab button on the left if you have not done so.

<img src="/Labs-ElasticCloud/images/virtual_classroom_user_guide_lab-terminal.png" alt="virtual_class" width="500" height="300">

2. When the terminal comes up, you should be at `/home/ubuntu`

3. Go to the metricbeat directory, `cd elastic/metricbeat-7.4.1-linux-x86_64/`
4. Use your favorite text editor to open `metricbeat.yml`, replace `cloud.id` and `cloud.auth` with the values you have saved earlier, and save the file.

<img src="/Labs-ElasticCloud/images/img18.png" alt="virtual_class" width="750" height="200">

5. Start metricbeat, `./metricbeat -e`

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

6. Use your favorite editor to edit `modules.d/nginx.yml`

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

Please note, you only need to copy and paste the section starting from var.paths. Again, use `keyboard control o` to save the file and `keyboard control x` to exit.  

4. Use your favorite text editor to open `filebeat.yml`, replace `cloud.id` and `cloud.auth` with the values you have saved earlier, and save the file.

<img src="/Labs-ElasticCloud/images/img18.png" alt="virtual_class" width="750" height="200">

10. Finally, we are ready to start filebeat `./filebeat -e`

### Validate the data in Kibana.

Let's take a look at what we have done so far.

1. Go back to the browser.
2. Go to the dashboards.

<img src="/Labs/images/dashboards.png" width="400">

5. Type in `metricbeat system` in the search bar and hit enter.
6. Select `[Metricbeat System] ECS Overview`
7. Click on Dashboards to go back.

<img src="/Labs/images/dashboards1.png" width="400">

8. In the searchbox, type in `filebeat nginx`
9. Select `[Filebeat Nginx] Overview ECS`
10. Be sure to change the date to last 1 year and click the Refresh button.

<img src="/Labs/images/dashboard2.png" width="400" height="250">

### Infra and Log UI

Within Kibana, there are two plugins that are dedicated to Infrastructure and Logs.  

1. Click on the Infrastructure icon to go to the Infra UI.

<img src="/Labs/images/InfraUI.png" width="200" >

2. Click on the host to bring up the pop up menu

<img src="/Labs/images/infraUI1.png" width="500" >

3. Click on View logs to go to the Logs UI or View metrics to go to the Metric UI

This completes Lab2!
