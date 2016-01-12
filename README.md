# pmcli

Multi-platform Marathon CLI written in python.

## Installation

Needs a python interpreter (tested in 2.x) and this modules:

- ConfigParser
- requests
- json

Just copy 'pmcli' file under any of your system's paths 

## Configuration

You can supply a configuration file from the CLI or use the default one under ~./.pmcli.cfg. The format is:

```
# Marathon server DNS name or IP
host = [marathon Address]

# Marathon port
port = [Port]

# Marathon username (in case of authentication)
user = [Username]

# Maraton password (in case of authentication)
password = [Password]

# Output format
#       human  (simplified columns)
#       json   (json on one line)
#       jsonpp (json pretty printed, default)
#       raw    (unprocessed reply from Marathon)
format = [Output Format]

# HTTP proxy addres. Format http://<name>:<port>
proxy = [Proxy Address]

# HTTPS proxy addres. Format https://<name>:<port>
proxys = [SSL Proxy Address]

# Timeout for API connections, in seconds
timeout = [Timeout]
```
You can have different marathon hosts (profiles) in the file and specify the one to use from the CLI with the '-p' switch

```
[default]
host = <marathonhost1>
user = <username1>
password = <password1>
format = <output format1>
port = <marathon port1>
proxy = <marathon proxy1>
proxys = <marathon ssl proxy1>
timeout = <timeout2>

[profile2]
host = <marathonhost2>
user = <username2>
password = <password2>
format = <output format2>
port = <marathon port2>
proxy = <marathon proxy2>
proxys = <marathon ssl proxy2>
timeout = <timeout2>
```

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
    │      ├─ change [opt] [appid] [value]    - change appid option 'opt' to 'value'
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
              └─┬─ leader    - (v0.7.0 and above) get the current Marathon leader
                ├─ abdicate  - (v0.7.0 and above) force the current leader to relinquish control
                ├─ ping      - ping Marathon master host[s]
                ├─ events    - (v0.9.0 and above) Attach to the events stream (exit with [Ctrl + C])
                ├─ info      - (v0.7.0 and above) get info about marathon instance
                └─ metrics   - get marathon metrics
 Flags
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
  -F Force operation

```

## TO DO

- Support SSL
- Share it from a docker container
- Test it with python 3.x
- Test it in other platform different than linux

I'm opened to any suggestion

## Personal web page

[Cooking devops](http://cookingdevops.blogspot.com)


