# pmcli

Multi-platform Marathon CLI written in python.

## What’s new in pmcli 1.1

- Attach to events stream with filter options
- SSL support
- Manage Subscriptions
- Native proxy support
- User defined timeout to API calls
- Option to manually reset applications launch delay
- Bug fixed and code cleaning

## Installation

Needs a python interpreter (tested in 2.x) and the following modules:

- ConfigParser
- requests
- json

Just copy 'pmcli' file under any of your system's paths.

## Usage

```
pmcli <flags...> [section] [action]
    ├─ app
    │    └─┬─ list                            - list all apps
    │      ├─ versions [appid]                - list all versions of 'appid'
    │      ├─ show [appid]                    - show config and status of 'appid' (latest version)
    │      ├─ showversion [appid] [versionid] - show config and status of 'appid' and 'versionid'
    │      ├─ create [jsonfile]               - deploy application defined in jsonfile
    │      ├─ update [appid] [jsonfile]       - update application 'appid' as defined in jsonfile
    │      ├─ change [appid] [opt] [value]    - change appid option 'opt' to 'value'
    │      ├─ scale [appid] [N]               - Scale application 'appid' to have N instances
    │      ├─ restart [appid]                 - restart app of 'appid'
    │      ├─ destroy [appid]                 - destroy and remove all instances of 'appid'
    │      ├─ queue                           - (v0.7.0 and above) list all queued tasks
    │      └─ reset [appid]                   - (v0.10.0 and above) reset 'appid' launch delay
    │
    ├─ task
    │     └─┬─ list                       - list all tasks
    │       ├─ show [appid]               - list tasks of app of 'appid'
    │       ├─ kill [appid]               - kill all tasks of 'appid'
    │       └─ killtask [appid] [taskid]  - kill task 'taskid' of app 'appid'
    │
    ├─ group (v0.7.0 and above)
    │      └─┬─ list                        - list all groups
    │        ├─ show [groupid]              - list groups in 'groupid'
    │        ├─ create [jsonfile]           - create a group defined in jsonfile
    │        ├─ update [groupid] [jsonfile] - update 'groupid' as defined in jsonfile
    │        └─ destroy [groupid]           - destroy group of 'groupid'
    │
    ├─ deploy (v0.7.0 and above)
    │       └─┬─ list                - list all active deploys
    │         └─ destroy [deployid]  - cancel deployment of 'deployid'
    │
    ├─ subs (v0.9.0 and above)
    │     └─┬─ list              - List all event subscriber callback URLs
    │       ├─ register [url]    - Register 'url' as an event subscriber list
    │       └─ unregister [url]  - Unregister 'url' from the event subscribers list
    │
    └─ marathon
              └─┬─ leader          - (v0.7.0 and above) get the current Marathon leader
                ├─ abdicate        - (v0.7.0 and above) force the current leader to relinquish control
                ├─ ping            - ping Marathon master host[s]
                ├─ events [filter] - (v0.9.0 and above) Attach to the events stream (exit with [Ctrl + C])
                │                    filter (optional):   e[[comma separated events]]
                │                                         a[[id]:[value]]
                ├─ info            - (v0.7.0 and above) get info about marathon instance
                └─ metrics         - get marathon metrics
 Flags
  -usage (prints usage)
  -c [config file]
  -h [host]
  -P [port] Marathon port
  -u [user:password] (separated by colon)
  -p [profile] (profile used in the config file)
  -o [output file] (json format. Overrides -f flag)
  -f [format]
       human  (simplified columns)
       json   (json on one line)
       jsonpp (json pretty printed, default)
       raw    (unprocessed reply from Marathon)
  -t [timeout] Timeout for connections
  -x [http proxy] proxy address
  -X [https proxy] SSL proxy address
  -S Use SSL
  -N Do not verify SSL
  -C Certificate file (pem format)
  -F Force operation

```

## Using config file

The connection parameters can be supplied by arguments or using a config file. By default this config file is ~./.pmcli.cfg or can be supplied to command line with the ‘-c’ switch.

The format and options used by this config file are the following ones:

```
[default]

# Marathon server DNS name or IP
host = [marathon Address]

# Marathon port. (default: 8080)
port = [Port]

# Use SSL. 1 -> True (default: 0)
ssl = [1:0]

# Verify SSL certificate. 1 -> True, 0 -> False (default: 1)
# Disable SSL certificate verification IS NOT RECOMMENDED
verifyssl = 1

# Path to SSL certificate. 1 in case of no certificate verification
sslcert = [certificate file]

# Marathon username (in case of authentication)
user = [Username]

# Maraton password (in case of authentication)
password = [Password]

# Output format (default: jsonpp)
#       human  (simplified columns)
#       json   (json on one line)
#       jsonpp (json pretty printed, default)
#       raw    (unprocessed reply from Marathon)
format = [Output Format]

# HTTP proxy addres. Format http://<name>:<port>
proxy = [Proxy Address]

# HTTPS proxy addres. Format https://<name>:<port>
sproxy = [SSL Proxy Address]

# Timeout for API connections (default: 60)
timeout = [Timeout]
```
You can have different marathon hosts (profiles) in the file and specify the one to use from the CLI with the '-p' switch

```
[default]
host = <marathonhost1>
ssl = <ssl1>
verifyssl = <verifyssl1>
cert = <cert1>
user = <username1>
password = <password1>
format = <output format1>
port = <marathon port1>
proxy = <marathon proxy1>
proxys = <marathon ssl proxy1>
timeout = <timeout2>

[profile2]
host = <marathonhost2>
ssl = <ssl2>
verifyssl = <verifyssl2>
cert = <cert2>
user = <username2>
password = <password2>
format = <output format2>
port = <marathon port2>
proxy = <marathon proxy2>
proxys = <marathon ssl proxy2>
timeout = <timeout2>
```

By default, and if ‘-p’ switch is not used, pmcli will use the default profile. One example would be:
```
	$ pmcli app list (uses default profile)
	# pmcli -p profile2 app list (uses profile2 profile)
```
## Filters

One interesting option in marathon 0.9.0 and above is events stream, where we can watch in real time all events triggered in marathon. This is realized by an http connection never closed by the server where all the events are sent to the client as soon as are generated in marathon.
This events are composed by two field: ‘event’ and ‘data’. Event it’s the event type generated and data it’s the content of this event. We can attach to this event stream by typing “pmcli marathon events”
```
$ pmcli marathon events
event: event_stream_attached
{"remoteAddress":"92.186.41.60","eventType":"event_stream_attached","timestamp":"2016-01-18T10:59:34.436Z"}

event: status_update_event
{"slaveId":"dfa6bede-c570-4e14-898c-5128d8e3da50-S0","taskId":"myapp.90766c83-bdd2-11e5-a8cb-02424144e2f6","taskStatus":"TASK_RUNNING","message":"","appId":"/myapp","host":"ip-172-31-26-90.eu-central-1.compute.internal","ports":[31732],"version":"2016-01-18T10:58:56.514Z","eventType":"status_update_event","timestamp":"2016-01-18T10:59:37.913Z"}

event: status_update_event
{"slaveId":"dfa6bede-c570-4e14-898c-5128d8e3da50-S0","taskId":"myapp.90766c83-bdd2-11e5-a8cb-02424144e2f6","taskStatus":"TASK_FINISHED","message":"","appId":"/myapp","host":"ip-172-31-26-90.eu-central-1.compute.internal","ports":[31732],"version":"2016-01-18T10:58:56.514Z","eventType":"status_update_event","timestamp":"2016-01-18T10:59:37.954Z"}
[..]
```
To close the connection the user must supply “Ctrl +C”
As we can see in the example, ALL the events are printed to the standard output. We can filter this from pmcli using filters.
```
filter by event type: e[[comma separated events]]
filter by data field : a[[id]:[value]]
```
For example, it we are only interested in “event_stream_attached” and “status_update_event” events, we can use the syntax:
```
$ pmcli marathon events e[status_update_event,event_stream_attached]
event: event_stream_attached
{"remoteAddress":"92.186.41.60","eventType":"event_stream_attached","timestamp":"2016-01-18T11:01:15.065Z"}

event: status_update_event
{"slaveId":"dfa6bede-c570-4e14-898c-5128d8e3da50-S0","taskId":"myapp.cc96ba47-bdd2-11e5-a8cb-02424144e2f6","taskStatus":"TASK_RUNNING","message":"","appId":"/myapp","host":"ip-172-31-26-90.eu-central-1.compute.internal","ports":[31443],"version":"2016-01-18T10:58:56.514Z","eventType":"status_update_event","timestamp":"2016-01-18T11:01:18.681Z"}

event: status_update_event
{"slaveId":"dfa6bede-c570-4e14-898c-5128d8e3da50-S0","taskId":"myapp.cc96ba47-bdd2-11e5-a8cb-02424144e2f6","taskStatus":"TASK_FINISHED","message":"","appId":"/myapp","host":"ip-172-31-26-90.eu-central-1.compute.internal","ports":[31443],"version":"2016-01-18T10:58:56.514Z","eventType":"status_update_event","timestamp":"2016-01-18T11:01:18.757Z"}

event: status_update_event
{"slaveId":"dfa6bede-c570-4e14-898c-5128d8e3da50-S0","taskId":"myapp.cf92bc38-bdd2-11e5-a8cb-02424144e2f6","taskStatus":"TASK_RUNNING","message":"","appId":"/myapp","host":"ip-172-31-26-90.eu-central-1.compute.internal","ports":[31858],"version":"2016-01-18T10:58:56.514Z","eventType":"status_update_event","timestamp":"2016-01-18T11:01:24.757Z"}
	[..]
```

We can also filter by data field. Suppose we are only interested in “myapp” events, so the syntax would be:
```
$ pmcli marathon events a[appId:/myapp]
event: status_update_event
{"slaveId":"dfa6bede-c570-4e14-898c-5128d8e3da50-S0","taskId":"myapp.3af853c1-bdd3-11e5-a8cb-02424144e2f6","taskStatus":"TASK_RUNNING","message":"","appId":"/myapp","host":"ip-172-31-26-90.eu-central-1.compute.internal","ports":[31754],"version":"2016-01-18T11:04:11.695Z","eventType":"status_update_event","timestamp":"2016-01-18T11:04:25.332Z"}
[..]
```
Right now only one level in json output file can be filtered. Support for more json levels filtering support and use of logical operands with this filters is my actual objective for the next version of pmcli.

Events stream output support the four different output type: json, jsonpp, human and raw.
```
$ pmcli -f jsonpp marathon events a[appId:/myapp2]
event: status_update_event
{
   "timestamp": "2016-01-18T12:21:18.512Z",
   "eventType": "status_update_event",
   "taskStatus": "TASK_RUNNING",
   "host": "ip-172-31-17-20.eu-central-1.compute.internal",
   "version": "2016-01-18T12:21:18.175Z",
   "taskId": "myapp2.f9eba497-bddd-11e5-a8a6-024270636925",
   "appId": "/myapp2",
   "message": "",
   "slaveId": "ecae0a10-70fa-48e3-94ea-ee6bed1c5ca2-S0",
   "ports": [    31862  ]
}
```
## Subscriptions

Another interesting marathon's functionality it's to send events to specific URL to advise system's about applications changes. This allow us to dynamically reconfigure our systems very time a change occurs within applications in marathon. For example a "scale" event can talk to a front end proxy to reconfigure for the new web servers in marathon.

This URL can be now manage from pmcli if marathon daemon is working with callback support  ```
$ pmcli subs register http://proxyp_ip/events
{ "eventType": "subscribe_event",
"clientIp": "92.186.41.60",
"timestamp": "2016-01-18T12:29:46.693Z",
"callbackUrl": "http://proxyp_ip/events"}

$ pmcli subs list
{ "callbackUrls": [ "http://proxyp_ip/events" ]}

$ pmcli subs unregister http://proxyp_ip/events
Do you really want to unregister 'http://proxyp_ip/events' ? (y/N)y{ "eventType": "unsubscribe_event",
"clientIp": "92.186.41.60",
"timestamp": "2016-01-18T12:30:15.628Z",
"callbackUrl": "http://proxyp_ip/events"}
```
## SSL

Due to security reason it's very important our connection to marathon works with an encrypted and authenticated sessions.

With pmcli v1.1 now it's possible to connect to protected marathon instances using '-S' switch. Also we can supply a certificate for the connection.
```
 $ pmcli -S -C certificate.pem app list
 ```
This can also be configured from config file.

We can use SSL without certificate verification with '-N' argument, but this is not recommended for security implications. Also remember to change marathon port, by default plain http uses 8080 and ssl uses 8443.

## Proxy support

As marathon api uses a RESTful API, connections from environments using proxy filtering needs to know where is this proxy. From Linux systems it's possible to define it with http_proxy and https_proxy, but pmcli now uses it's own proxy configuration. This can be done from arguments or from configuration file
```
$ pmcli -x http://[proxy]:[port] -X https://[ssl proxy]:[port] app list
```

## TO DO

- Option to filter event stream with keys with more than 2 levels depth
- Unicode support
- Share it from a docker container
- Test it with python 3.x
- Test it in other platform different than linux

I'm opened to any suggestion

## Personal web page

[Cooking devops](http://cookingdevops.blogspot.com)
