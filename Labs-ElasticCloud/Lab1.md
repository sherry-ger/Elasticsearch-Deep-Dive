# Create your Elastic Cloud environment

## Overview

* Setup Elasticsearch
* Setup Kibana

### Setup Elasticsearch

1. Sign up for the Elastic Cloud Trial

Visit [https://cloud.elastic.co](https://cloud.elastic.co) and click “Sign up now”

<img src="/Labs-ElasticCloud/images/img1.png" alt="virtual_class" width="500" height="200">

2. Enter your business email (no credit card is required)

<img src="/Labs-ElasticCloud/images/img2.png" alt="virtual_class" width="300" height="200">

3. Check your email to verify your account. (sometimes it may take a few minutes before email shows up in inbox)

<img src="/Labs-ElasticCloud/images/img3.png" alt="virtual_class" width="350" height="300">

_Occasionally we have seen issues with browsers - usually email not showing up. If it’s not going the way as the lab points out - please try to use another browser._
_Corporate email servers have blocked the cloud trial emails, a non-corporate email may do the trick_

4. Log in and create your account a strong password.

<img src="/Labs-ElasticCloud/images/img4.png" alt="virtual_class" width="300" height="200">

5.  You should now be looking at the Welcome Screen for your trial account.  Click “Create deployment”.

<img src="/Labs-ElasticCloud/images/img5.png" alt="virtual_class" width="500" height="300">

6.  Give your deployment a name. 

<img src="/Labs-ElasticCloud/images/img6.png" alt="virtual_class" width="350" height="150">

7.   You have the choice here to pick the cloud platform and region of your choice. For the labs let the default region be the pick. 

<img src="/Labs-ElasticCloud/images/img7.png" alt="virtual_class" width="700" height="300">

8.  Pick the latest version of Elastic.

<img src="/Labs-ElasticCloud/images/SetupDeployment.png" alt="virtual_class" width="500" height="200">

9.  Select the I/O Optimized template

<img src="/Labs-ElasticCloud/images/img8.png" alt="virtual_class" width="800" height="300">

10.  Click “Customize deployment”.  We want to make a few more changes before creating the deployment.

<img src="/Labs-ElasticCloud/images/img9.png" alt="virtual_class" width="400" height="200">

11. You should see 2 Data nodes that will be deployed across 2 zones.  During the trial, you will not be able to change the size of them. If this wasn’t a trial account, changing the size of the ES nodes or the # of nodes is a slider-away!

<img src="/Labs-ElasticCloud/images/img10.png" alt="virtual_class" width="500" height="300">

12.  Turn on <code>Machine Learning</code></strong> by clicking <strong><code>Enable</code></strong>.

<img src="/Labs-ElasticCloud/images/img11.png" alt="virtual_class" width="400" height="150">

Your cluster’s Architecture should look similar to this. Notice that APM server is part of the deployment and hosted for use as well.

<img src="/Labs-ElasticCloud/images/img12.png" alt="virtual_class" width="500" height="500">

13.  Click <code>Create Cluster</code></strong>. Your cluster will begin to spin up.  It takes between 3 - 6 minutes to complete.

<img src="/Labs-ElasticCloud/images/img13.png" alt="virtual_class" width="300" height="50">

Please copy **Cloud ID, username, auto generated password**. Please paste it into a text editor of your choice for now.  \

<img src="/Labs-ElasticCloud/images/img14.png" alt="virtual_class" width="600" height="300">

14.  When the cluster is ready, select your deployment in the top left, under **Deployments**. Scroll down to get used to having all information about your deployment at a single place.

<img src="/Labs-ElasticCloud/images/img15.png" alt="virtual_class" width="400" height="150">


This is where you can find the URLs for Elasticsearch, Kibana, and APM. 

15.  Click on <code>Launch</code></strong> under Kibana to launch it in a new tab.  Login with the username <strong><code>elastic</code></strong> and your password (this is the password you copied in Step 13).

<img src="/Labs-ElasticCloud/images/img16.png" alt="virtual_class" width="250" height="200">

**_[Note: If you forgot to copy your password earlier, you can always reset it to get a new password under the Security section of your deployment]_**

<img src="/Labs-ElasticCloud/images/img17.png" alt="virtual_class" width="500" height="300">

That’s it for this lab! You have a deployment now ready to start ingesting and working with the data. 

