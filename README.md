# pmcli
Marathon CLI written in python. Based on Marathonctl

## Installation

Just needs a python interpreter (2.x or 3.x) and this very common python modules:

- sys
- os
- termios
- ConfigParser
- requests
- io
- json

## Configuration

You can supply a configuration file from the CLI or use the default one under ~./.pmcli.cfg. The format is:

[default]
host = <marathonhost>
user = <username>
password = <password>
format = <output format>
port = <marathon port>

format can be:

* human  (simplified columns)
* json   (json on one line)
* jsonpp (json pretty printed, default)
* raw    (the exact response from Marathon)

You can have different marathon hosts in the file and specify it from the CLI with the '-p' switch

[default]
host = <marathonhost1>
user = <username1>
password = <password1>
format = <output format1>
port = <marathon port1>

[profile2]
host = <marathonhost2>
user = <username2>
password = <password2>
format = <output format2>
port = <marathon port2>

## Usage

 <flags...> [section] [action]
    ├─ app
    │    └─┬─ list                          - list all apps
    │      ├─ versions [appid]              - list all versions of apps of appid
    │      ├─ show [appid]                  - show config and status of app of appid (latest version)
    │      ├─ show [appid] [version]        - show config and status of app of appid and version
    │      ├─ create [jsonfile]             - deploy application defined in jsonfile
    │      ├─ update [appid] [jsonfile]     - update application appid as defined in jsonfile
    │      ├─ update cpu [appid] [cpu%]     - update application appid to have cpu% of cpu share
    │      ├─ update memory [appid] [MB]    - update application appid to have MB of memory
    │      ├─ update instances [appid] [N]  - update application appid to have N instances
    │      ├─ restart [appid]               - restart app of appid
    │      └─ destroy [appid]               - destroy and remove all instances of appid
    │
    ├─ task
    │     └─┬─ list                       - list all tasks
    │       ├─ list [appid]               - list tasks of app of appid
    │       ├─ kill [appid]               - kill all tasks of app appid
    │       ├─ killtask [appid] [taskid]  - kill task taskid of app appid
    │       └─ queue                      - list all queued tasks
    │
    ├─ group
    │      └─┬─ list                        - list all groups
    │        ├─ list [groupid]              - list groups in groupid
    │        ├─ create [jsonfile]           - create a group defined in jsonfile
    │        ├─ update [groupid] [jsonfile] - update group groupid as defined in jsonfile
    │        └─ destroy [groupid]           - destroy group of groupid
    │
    ├─ deploy
    │       └─┬─ list                - list all active deploys
    │         └─ destroy [deployid]  - cancel deployment of [deployid]
    │
    └─ marathon
              └─┬─ leader    - get the current Marathon leader
                ├─ abdicate  - force the current leader to relinquish control
                ├─ ping      - ping Marathon master host[s]
                ├─ info      - get info about marathon instance
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
       raw    (the exact response from Marathon)
  -F Force operation

## TO DO

- Update resources from command line
- Events subscriptions (?)
- Option to reset launch delay in an app
- Events stream (?)
- Share it from a docker container

I'm open to any suggestion

## Known bugs

Still doesn't works "pmcli app update [option]". This will be my first priority in the developement

## Web page

[cookingdevops.blogspot.com]


