# pmcli
Marathon CLI written in python. Based on Marathonctl

You can supply a configuration file from the CLI or use the default one under ~./.pmcli.cfg. The format is:

[default]
host = <marathonhost>
authentication = 1
user = testuser
password = testpassword
format = jsonpp

You can have different marathon hosts in the file and specify it from the CLI with the '-p' switch

[default]
host = <marathonhost>
authentication = 1
user = testuser
password = testpassword
format = jsonpp

[marathon2]
host = <marathonhost2>
authentication = 1
user = testuser
password = testpassword
format = jsonpp

Usage:

pmcli <flags...> [action] <args...>
 Actions
    app
       list                      - list all apps
       versions [id]             - list all versions of apps of id
       show [id]                 - show config and status of app of id (latest version)
       show [id] [version]       - show config and status of app of id and version
       create [jsonfile]         - deploy application defined in jsonfile
       update [id] [jsonfile]    - update application id as defined in jsonfile
       update cpu [id] [cpu%]    - update application id to have cpu% of cpu share
       update memory [id] [MB]   - update application id to have MB of memory
       update instances [id] [N] - update application id to have N instances
       restart [id]              - restart app of id
       destroy [id]              - destroy and remove all instances of id

    task
       list               - list all tasks
       list [id]          - list tasks of app of id
       kill [id]          - kill all tasks of app id
       kill [id] [taskid] - kill task taskid of app id
       queue              - list all queued tasks

    group
       list                        - list all groups
       list [groupid]              - list groups in groupid
       create [jsonfile]           - create a group defined in jsonfile
       update [groupid] [jsonfile] - update group groupid as defined in jsonfile
       destroy [groupid]           - destroy group of groupid

    deploy
       list               - list all active deploys
       destroy [deployid] - cancel deployment of [deployid]

    marathon
       leader   - get the current Marathon leader
       abdicate - force the current leader to relinquish control
       ping     - ping Marathon master host[s]

 Flags
  -c [config file]
  -h [host]
  -u [user:password] (separated by colon)
  -p [profile] (profile used in the config file)
  -f [format]
       human  (simplified columns, default)
       json   (json on one line)
       jsonpp (json pretty printed)
       raw    (the exact response from Marathon)

