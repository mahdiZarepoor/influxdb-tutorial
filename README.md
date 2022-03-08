# influxdb-tutorial
1) ## Install influxdb2 on Ubuntu 

### adding gpg key using this command
`wget -qO- https://repos.influxdata.com/influxdb.key | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/influxdb.gpg > /dev/null`

### add the repo
`export DISTRIB_ID=$(lsb_release -si); export DISTRIB_CODENAME=$(lsb_release -sc)
echo "deb [signed-by=/etc/apt/trusted.gpg.d/influxdb.gpg] https://repos.influxdata.com/${DISTRIB_ID,,} ${DISTRIB_CODENAME} stable" | sudo tee /etc/apt/sources.list.d/influxdb.list > /dev/null`

### update repos

`sudo apt-get update`

### install influxdb2

`sudo apt-get install influxdb2`

### check
`:~$ apt-cache policy influxdb2
influxdb2:
  Installed: 2.1.1
  Candidate: 2.1.1
  Version table:
 *** 2.1.1 500
        500 https://repos.influxdata.com/ubuntu focal/stable amd64 Packages
        100 /var/lib/dpkg/status
`


## start and enable influxdb2

`sudo systemctl start influxdb`
`sudo systemctl enable influxdb`

`:~$ sudo systemctl status influxdb
● influxdb.service - InfluxDB is an open-source, distributed, time series database
     Loaded: loaded (/lib/systemd/system/influxdb.service; enabled; vendor preset: enabled)
     Active: active (running) since Mon 2022-03-07 08:13:25 +0330; 52min ago
       Docs: https://docs.influxdata.com/influxdb/
    Process: 1665 ExecStart=/usr/lib/influxdb/scripts/influxd-systemd-start.sh (code=exited, status=0/SUCCESS)
   Main PID: 1683 (influxd)
      Tasks: 15 (limit: 11715)
     Memory: 117.4M
     CGroup: /system.slice/influxdb.service
             └─1683 /usr/bin/influxd
`



2) ## installing influx (cli client)
`sudo apt install -y influxdb2-cli`
### check :
`:~$ apt-cache policy influxdb2-cli 
influxdb2-cli:
  Installed: 2.2.1
  Candidate: 2.2.1
  Version table:
 *** 2.2.1 500
        500 https://repos.influxdata.com/ubuntu focal/stable amd64 Packages
        100 /var/lib/dpkg/status
`


3) ## setup influxdb : creating a default organization, user, bucket, and Operator API token


### Set up InfluxDB through the UI

    With InfluxDB running, visit localhost:8086.
    Click Get Started

### Set up your initial user

    Enter a Username for your initial user.
    Enter a Password and Confirm Password for your user.
    Enter your initial Organization Name.
    Enter your initial Bucket Name.
    Click Continue.

### set up InfluxDB using influx CLI
`influx setup`

    - Enter a primary username.
    - Enter a password for your user.
    - Confirm your password by entering it again.
    - Enter a name for your primary organization.
    - Enter a name for your primary bucket.
    - Enter a retention period for your primary bucket 
    The part of InfluxDB’s data structure that describes for how long InfluxDB keeps data (duration), how many copies of those data are stored in the cluster (replication factor), and the time range covered by shard groups (shard group duration).RPs are unique per database and along with the measurement and tag set define a series
    When you create a database, InfluxDB automatically creates a retention policy called autogen with an infinite duration, a replication factor set to one, and a shard group duration set to seven days.


### manual configuration :
**you need getting token**
search in your borwser : http://localhost:8086 
login into your UI using username and password you set > data > API token 
run the command below 
`influx config create --config-name <your_config_name> --host-url <url> --token <> --active) `

## some common commands you may need 

### creating an organization
`influx org create -n org_name 
`
### 
`influx bucket create --org <org_name> --name <bucket_name> --retention 4380h
`
### to list buckets
`influx bucket list 
`
### to create user
`influx user create --name <user_name> --org <org_name> --password SecretP4ss
`
### set authorization
`influx auth create --user <username> --read-buckets --write-buckets
`



### link for sample dataset 
https://raw.githubusercontent.com/influxdata/influxdb2-sample-data/master/air-sensor-data/air-sensor-data.lp

### command to import (write from a file)
`influx write --bucket <bucket_name> --file <path_to_file> --format <file_format>`


good luck :) 
