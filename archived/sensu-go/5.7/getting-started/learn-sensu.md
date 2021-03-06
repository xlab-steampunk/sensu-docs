---
title: "Learn Sensu Go"
description: "Here’s everything you need to start learning Sensu Go, including how to set up our sandbox and your first three lesson plans. You'll learn how to create a monitoring event and event pipeline, as well as automate event production with the Sensu agent."
version: "5.7"
product: "Sensu Go"
---

In this tutorial, we'll download the Sensu sandbox and create a monitoring workflow with Sensu.

- [Set up the sandbox](#set-up-the-sandbox)
- [Lesson \#1: Create a monitoring event](#lesson-1-create-a-sensu-monitoring-event)
- [Lesson \#2: Create an event pipeline](#lesson-2-pipe-keepalive-events-into-slack)
- [Lesson \#3: Automate event production with the Sensu agent](#lesson-3-automate-event-production-with-the-sensu-agent)

---

In this tutorial, you'll download the Sensu sandbox and create a monitoring workflow with Sensu.

- [Set up the sandbox](#set-up-the-sandbox)
- [Lesson \#1: Create a monitoring event](#lesson-1-create-a-sensu-monitoring-event)
- [Lesson \#2: Create an event pipeline](#lesson-2-pipe-keepalive-events-into-slack)
- [Lesson \#3: Automate event production with the Sensu agent](#lesson-3-automate-event-production-with-the-sensu-agent)

---

## Set up the sandbox

**1. Install Vagrant and VirtualBox**

- [Download Vagrant](https://www.vagrantup.com/downloads.html)
- [Download VirtualBox](https://www.virtualbox.org/wiki/Downloads)

**2. Download the sandbox**

[Download from GitHub](https://github.com/sensu/sandbox/archive/master.zip) or clone the repository:

{{< highlight shell >}}
git clone https://github.com/sensu/sandbox && cd sandbox/sensu-go
{{< /highlight >}}

_**NOTE**: If you've cloned the sandbox repository before, run `cd sandbox/sensu-go` and `git pull https://github.com/sensu/sandbox` instead._

**3. Start Vagrant**

{{< highlight shell >}}
ENABLE_SENSU_SANDBOX_PORT_FORWARDING=1 vagrant up
{{< /highlight >}}

The Learn Sensu sandbox is a CentOS 7 virtual machine pre-installed with Sensu, InfluxDB, and Grafana.
It's intended for you to use as a learning tool &mdash; we do not recommend using it in a production installation. To install Sensu in production, use the [installation guide](../../installation/install-sensu) instead.

The sandbox startup process takes about 5 minutes.

_**NOTE**: The sandbox configures VirtualBox to forward TCP ports 3002 and 4002 from the sandbox virtual machine (VM) to the localhost to make it easier for you to interact with the sandbox dashboards. Dashboard links provided in this tutorial assume port forwarding from the VM to the host is active._

**4. SSH into the sandbox**

Thanks for waiting! To start shell into the sandbox:

{{< highlight shell >}}
vagrant ssh
{{< /highlight >}}

You should be greeted with this prompt:  

{{< highlight shell >}}
[sensu_go_sandbox]$
{{< /highlight >}}

- To exit out of the sandbox, press `CTRL`+`D`.
- To erase and restart the sandbox, run `vagrant destroy` and then `vagrant up`.
- To reset the sandbox's Sensu configuration to the beginning of this tutorial, run `vagrant provision`.

_NOTE: The sandbox pre-configures sensuctl with the Sensu Go admin user, so you won't have to configure sensuctl each time you spin up the sandbox to try out a new feature. Before installing sensuctl outside of the sandbox, read the [first time setup reference][1] to learn how to configure sensuctl._  

---

## Lesson \#1: Create a Sensu monitoring event

First, make sure everything is working correctly using the sensuctl command line tool. Use sensuctl to see that your Sensu backend instance has a single namespace, `default`, and two users: the default admin user and the user created for a Sensu agent to use.

{{< highlight shell >}}
sensuctl namespace list
{{< /highlight >}}

{{< highlight shell >}}
  Name    
─────────
 default  
{{< /highlight >}}

{{< highlight shell >}}
sensuctl user list
{{< /highlight >}}

{{< highlight shell >}}
 Username       Groups       Enabled  
────────── ──────────────── ───────── 
admin      cluster-admins   true     
agent      system:agents    true    
{{< /highlight >}}

Sensu keeps track of monitored components as entities. Start by using sensuctl to make sure Sensu hasn't connected to any entities yet:

{{< highlight shell >}}
sensuctl entity list
{{< /highlight >}}

{{< highlight shell >}}
 ID   Class   OS   Subscriptions   Last Seen  
──── ─────── ──── ─────────────── ─────────── 
{{< /highlight >}}

Now you can start the Sensu agent to begin monitoring the sandbox:

{{< highlight shell >}}
sudo systemctl start sensu-agent
{{< /highlight >}}

Use sensuctl to see that Sensu is now monitoring the sandbox entity:

{{< highlight shell >}}
sensuctl entity list
{{< /highlight >}}

{{< highlight shell >}}
        ID          Class    OS          Subscriptions                  Last Seen            
────────────────── ─────── ─────── ───────────────────────── ─────────────────────────────── 
sensu-go-sandbox   agent   linux   entity:sensu-go-sandbox   2019-01-24 21:29:06 +0000 UTC  
{{< /highlight >}}

Sensu agents send keepalive events to help you monitor agent status. Use sensuctl to see the keepalive events generated by the sandbox entity:

{{< highlight shell >}}
sensuctl event list
{{< /highlight >}}

{{< highlight shell >}}
      Entity          Check                                       Output                                     Status   Silenced             Timestamp            
────────────────── ─────────── ──────────────────────────────────────────────────────────────────────────── ──────── ────────── ─────────────────────────────── 
sensu-go-sandbox   keepalive   Keepalive last sent from sensu-go-sandbox at 2019-01-24 21:29:06 +0000 UTC        0   false      2019-01-24 21:29:06 +0000 UTC 
{{< /highlight >}}

The sensu-go-sandbox keepalive event has status 0, which means the agent is in an OK state and is able to communicate with the Sensu backend.

You can also see the event and the entity in the [Sensu dashboard](http://localhost:3002). Log in to the dashboard with the default admin credentials: username `admin` and password `P@ssw0rd!`.

## Lesson \#2: Pipe keepalive events into Slack

Now that you know the sandbox is working properly, let's get to the fun stuff: creating a workflow. In this lesson, you'll create a workflow that sends keepalive alerts to Slack.

_**NOTE**: If you'd rather not create a Slack account, you can skip ahead to [Lesson \#3](#lesson-3-automate-event-production-with-the-sensu-agent)._

**1. Get your Slack webhook URL**

[Create a Slack workspace][2] (or use an existing workspace, if you’re already a Slack admin).

Then, visit `YOUR-WORKSPACE-NAME.slack.com/services/new/incoming-webhook`. Follow the steps to add the *Incoming WebHooks* integration and save your webhook. Your webhook channel and URL will be listed under Integration Settings &mdash; you’ll need both later in this lesson.

**2. Register the Sensu Slack handler asset**

[Assets][3] are shareable, reusable packages that make it easy to deploy Sensu plugins. In this lesson, we'll use the [Sensu Slack handler asset][4] to power a `slack` handler.

Use sensuctl to register the [Sensu Slack handler asset][4].

{{< highlight shell >}}
sensuctl asset create sensu-slack-handler --url "https://assets.bonsai.sensu.io/3149de09525d5e042a83edbb6eb46152b02b5a65/sensu-slack-handler_1.0.3_linux_amd64.tar.gz" --sha512 "68720865127fbc7c2fe16ca4d7bbf2a187a2df703f4b4acae1c93e8a66556e9079e1270521999b5871473e6c851f51b34097c54fdb8d18eedb7064df9019adc8"
{{< /highlight >}}

You should see a confirmation message from sensuctl.

{{< highlight shell >}}
Created
{{< /highlight >}}

The `sensu-slack-handler` asset is now ready to use with Sensu.  Use sensuctl to see the complete asset definition.

{{< highlight shell >}}
sensuctl asset info sensu-slack-handler --format yaml
{{< /highlight >}}

_PRO TIP: You can use resource definitions to create and update resources (like assets) using `sensuctl create --file filename.yaml`. See the [sensuctl docs][5] for more information._

**3. Create a Sensu Slack handler**

Open the `sensu-slack-handler.json` handler definition provided with the sandbox in your preferred text editor. Edit the definition to include your Slack channel, webhook URL, and the `sensu-slack-handler` asset.

_**NOTE**: If you aren't sure how to open the handler and edit the definition, try these [Vi/Vim gist instructions][11]._

{{< highlight shell >}}
"env_vars": [
  "KEEPALIVE_SLACK_WEBHOOK=https://hooks.slack.com/services/AAA/BBB/CCC",
  "KEEPALIVE_SLACK_CHANNEL=#monitoring"
],
"runtime_assets": ["sensu-slack-handler"]
{{< /highlight >}}

Now you can create a Slack handler named `keepalive` to process keepalive events.

{{< highlight shell >}}
sensuctl create --file sensu-slack-handler.json
{{< /highlight >}}

Use sensuctl to see available event handlers &mdash; in this case, you'll only see the `keepalive` handler you just created..

{{< highlight shell >}}
sensuctl handler list
{{< /highlight >}}

{{< highlight shell >}}
  Name      Type   Timeout   Filters   Mutator                                                   Execute                                                                                                              Environment Variables                            Assets         
─────────── ────── ───────── ───────── ───────── ────────────────────────────────────────────────────────────────────────────────────────────────────────── ────────────────────────────────────────────────────────────────────────────────────────────────── ───────────────────── 
 keepalive   pipe         0                       RUN:  /usr/local/bin/sensu-slack-handler -c "${KEEPALIVE_SLACK_CHANNEL}" -w "${KEEPALIVE_SLACK_WEBHOOK}"   KEEPALIVE_SLACK_WEBHOOK=https://hooks.slack.com/services/AAA/BBB/CCC,KEEPALIVE_SLACK_CHANNEL=#monitoring   sensu-slack-handler  
{{< /highlight >}}

Sensu monitoring events should begin arriving in your Slack channel, indicating that the sandbox entity is in an OK state.

**4. Filter keepalive events**

Now that you're generating Slack alerts, let's reduce the potential for alert fatigue by adding a filter that sends only warning, critical, and resolution alerts to Slack.

To accomplish this, you'll interactively add the built-in is_incident filter to the keepalive handler, which will make sure you only receive alerts when the sandbox entity fails to send a keepalive event.

{{< highlight shell >}}
sensuctl handler update keepalive
{{< /highlight >}}

The first prompt will be for environment variables. Just press `return` to continue. The second prompt is for the filters selection &mdash; enter `is_incident` to apply the is_incident filter.

{{< highlight shell >}}
? Filters: is_incident
{{< /highlight >}}

For each of the mutator, timeout, type, runtime assets, and command prompts, just press `return`.

Use sensuctl to confirm that the keepalive handler now includes the is_incident filter:

{{< highlight shell >}}
sensuctl handler info keepalive
{{< /highlight >}}

{{< highlight shell >}}
=== keepalive
Name:                  keepalive
Type:                  pipe
Timeout:               0
Filters:               is_incident
Mutator:               
Execute:               RUN:  sensu-slack-handler -c "${KEEPALIVE_SLACK_CHANNEL}" -w "${KEEPALIVE_SLACK_WEBHOOK}"
Environment Variables: KEEPALIVE_SLACK_WEBHOOK=https://hooks.slack.com/services/AAA/BBB/CCC, KEEPALIVE_SLACK_CHANNEL=#monitoring
Runtime Assets:        sensu-slack-handler
{{< /highlight >}}

With the filter in place, you should no longer receive messages in your Slack channel every time the sandbox entity sends a keepalive event.

Let's stop the agent and confirm that you receive the expected warning message.

{{< highlight shell >}}
sudo systemctl stop sensu-agent
{{< /highlight >}}

After a couple minutes, you should see a warning message in your Slack channel informing you that the sandbox entity is no longer sending keepalive events.

Start the agent to resolve the warning.

{{< highlight shell >}}
sudo systemctl start sensu-agent
{{< /highlight >}}

## Lesson \#3: Automate event production with the Sensu agent

So far, you've used the Sensu agent's built-in keepalive feature, but in this lesson, you'll create a check that automatically produces workload-related events. Instead of sending alerts to Slack, you'll store event data with [InfluxDB](https://www.influxdata.com/) and visualize it with [Grafana](https://grafana.com/).

**1. Make sure the Sensu agent is running**

{{< highlight shell >}}
sudo systemctl restart sensu-agent
{{< /highlight >}}

**2. Install Nginx and the Sensu HTTP Plugin**

You'll use the [Sensu HTTP Plugin](https://github.com/sensu-plugins/sensu-plugins-http) to monitor an Nginx server running on the sandbox.

First, install the EPEL release package:

{{< highlight shell >}}
sudo yum install -y epel-release
{{< /highlight >}}

Then, install and start Nginx:

{{< highlight shell >}}
sudo yum install -y nginx && sudo systemctl start nginx
{{< /highlight >}}

Make sure it's working:

{{< highlight shell >}}
curl -I http://localhost:80
{{< /highlight >}}

{{< highlight shell >}}
HTTP/1.1 200 OK
...
{{< /highlight >}}

Then install the Sensu HTTP Plugin:

{{< highlight shell >}}
sudo sensu-install -p sensu-plugins-http
{{< /highlight >}}

You'll use the `metrics-curl.rb` plugin. Test its output with:

{{< highlight shell >}}
/opt/sensu-plugins-ruby/embedded/bin/metrics-curl.rb -u "http://localhost"
{{< /highlight >}}

{{< highlight shell >}}
...
sensu-go-sandbox.curl_timings.http_code 200 1535670975
{{< /highlight >}}


**3. Create an InfluxDB pipeline**

Now, let's create the InfluxDB pipeline to store these metrics and visualize them with Grafana. To create a pipeline to send metric events to InfluxDB, start by registering the [Sensu InfluxDB handler asset][6].

{{< highlight shell >}}
sensuctl asset create sensu-influxdb-handler --url "https://assets.bonsai.sensu.io/b28f8719a48aa8ea80c603f97e402975a98cea47/sensu-influxdb-handler_3.1.2_linux_amd64.tar.gz" --sha512 "612c6ff9928841090c4d23bf20aaf7558e4eed8977a848cf9e2899bb13a13e7540bac2b63e324f39d9b1257bb479676bc155b24e21bf93c722b812b0f15cb3bd"
{{< /highlight >}}

You should see a confirmation message from sensuctl.

{{< highlight shell >}}
Created
{{< /highlight >}}

The `sensu-influxdb-handler` asset is now ready to use with Sensu. Use sensuctl to see the complete asset definition.

{{< highlight shell >}}
sensuctl asset info sensu-influxdb-handler --format yaml
{{< /highlight >}}

Open the `influx-handler.json` handler definition provided with the sandbox, and edit the `runtime_assets` attribute to include the `sensu-influxdb-handler` asset.

{{< highlight shell >}}
"runtime_assets": ["sensu-influxdb-handler"]
{{< /highlight >}}

Now you can use sensuctl to create the `influx-db` handler:

{{< highlight shell >}}
sensuctl create --file influx-handler.json
{{< /highlight >}}

Use sensuctl to confirm that the handler was created successfully.

{{< highlight shell >}}
sensuctl handler list
{{< /highlight >}}

The `influx-db` handler should be listed. If you completed [lesson \#2](#lesson-2-pipe-keepalive-events-into-slack), you'll also see the `keepalive` handler.

**4. Create a check to monitor Nginx**

The `curl_timings-check.json` file provided with the sandbox will create a service check that runs the `metrics-curl.rb` check plugin every 10 seconds on all entities with the `entity:sensu-go-sandbox` subscription and sends events to the InfluxDB pipeline. The `metrics-curl.rb` plugin is already included as the value of the command field in `curl_timings-check.json` &mdash; you just need to create the file:

{{< highlight shell >}}
sensuctl create --file curl_timings-check.json
{{< /highlight >}}

{{< highlight shell >}}
sensuctl check list
{{< /highlight >}}

{{< highlight shell >}}
     Name                                        Command                                     Interval   Cron   Timeout   TTL        Subscriptions        Handlers   Assets   Hooks   Publish?   Stdin?     Metric Format      Metric Handlers  
────────────── ──────────────────────────────────────────────────────────────────────────── ────────── ────── ───────── ───── ───────────────────────── ────────── ──────── ─────── ────────── ──────── ──────────────────── ───────────────── 
curl_timings   /opt/sensu-plugins-ruby/embedded/bin/metrics-curl.rb -u "http://localhost"         10                0     0   entity:sensu-go-sandbox                               true       false    graphite_plaintext   influx-db        
{{< /highlight >}}

This check specifies a metrics handler and metric format. In Sensu Go, metrics are a core element of the data model: you can build pipelines to handle metrics separately from alerts. This allows you to customize your monitoring workflows to get better visibility and reduce alert fatigue.

After about 10 seconds, you can see the event produced by the entity:

{{< highlight shell >}}
sensuctl event info sensu-go-sandbox curl_timings --format json | jq .
{{< /highlight >}}

{{< highlight shell >}}
...
  "history": [
    {
      "status": 0,
      "executed": 1556472457
    },
  ],
  "output": "sensu-go-sandbox.curl_timings.time_total 0.005 1556472657\n...",
  ...
  "output_metric_format": "graphite_plaintext",
  "output_metric_handlers": [
    "influx-db"
  ],
...
{{< /highlight >}}

Because the check definition specified a metric format of `graphite_plaintext`, the Sensu agent will treat the output of the check command as Graphite-formatted metrics and translate them into a set of Sensu-formatted metrics (not shown in the output). These metrics are then sent to the InfluxDB handler, which reads Sensu-formatted metrics and converts them to a format InfluxDB accepts.

_**NOTE**: Metric support isn't limited to Graphite! The Sensu agent can extract metrics in multiple line protocol formats, including Nagios performance data._

**5. See the HTTP response code events for Nginx in Grafana.**

Log in to [Grafana](http://localhost:4002/d/go01/sensu-go-sandbox) with username: `admin` and password: `admin`. You should see a graph of live HTTP response codes for Nginx.

Now, if you turn Nginx off, you should see the impact in Grafana:

{{< highlight shell >}}
sudo systemctl stop nginx
{{< /highlight >}}

Start Nginx:

{{< highlight shell >}}
sudo systemctl start nginx
{{< /highlight >}}

**6. Automate disk usage monitoring for the sandbox**

Now that you have an entity set up, you can add more checks. For example, let's say you want to monitor disk usage on the sandbox.

First, install the plugin:

{{< highlight shell >}}
sudo sensu-install -p sensu-plugins-disk-checks
{{< /highlight >}}

Test the plugin:

{{< highlight shell >}}
/opt/sensu-plugins-ruby/embedded/bin/metrics-disk-usage.rb
{{< /highlight >}}

{{< highlight shell >}}
sensu-core-sandbox.disk_usage.root.used 2235 1534191189
sensu-core-sandbox.disk_usage.root.avail 39714 1534191189
...
{{< /highlight >}}

Then create the check using sensuctl and the `disk_usage-check.json` file included with the sandbox, assigning it to the `entity:sensu-go-sandbox` subscription and the InfluxDB pipeline:

{{< highlight shell >}}
sensuctl create --file disk_usage-check.json
{{< /highlight >}}

You don't need to make any changes to disk_usage-check.json before running `sensuctl create --file disk_usage-check.json`.

You should see the check working in the [dashboard entity view](http://localhost:3002/#/entities) and via sensuctl:

{{< highlight shell >}}
sensuctl event list
{{< /highlight >}}

Now, you should be able to see disk usage metrics for the sandbox in Grafana: [reload your Grafana tab to show the Sensu Go Sandbox Combined](http://localhost:4002/d/go02/sensu-go-sandbox-combined).

You made it! You're ready for the next level of Sensu-ing.

Before you move on, take a moment to remove the virtual machine and resources installed during this sandbox lesson. Press `CTRL`+`D` to exit the sandbox. Then run:
{{< highlight shell >}}
vagrant destroy
{{< /highlight >}}

Now you can continue exploring Sensu with a clean slate. Here are some resources to help continue your journey:

- Try another [lesson in the Sensu sandbox][7]
- [Install Sensu Go][8]
- [Collect StatsD metrics][9]
- [Create a read-only user][10]


[1]: ../../sensuctl/reference/#first-time-setup
[2]: https://slack.com/get-started#create
[3]: ../../reference/assets
[4]: https://bonsai.sensu.io/assets/sensu/sensu-slack-handler
[5]: ../../sensuctl/reference#creating-resources
[6]: https://bonsai.sensu.io/assets/sensu/sensu-influxdb-handler
[7]: ../../getting-started/sandbox/
[8]: ../../installation/install-sensu
[9]: ../../guides/aggregate-metrics-statsd
[10]: ../../guides/create-read-only-user/
[11]: https://gist.github.com/hillaryfraley/838a046821171b1a37d0dafb16584518
