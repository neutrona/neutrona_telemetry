# REQUIREMENTS (Ubuntu 16.04):

- Git
- Docker
- Docker Compose
- Docker UI (Portainer running on port 9000) - optional


```
#!/bin/bash
# as privileged user

apt update 
apt install git docker.io docker-compose

#optional:
docker run -d --privileged -p 9000:9000 --name portainer \
-v /var/run/docker.sock:/var/run/docker.sock \
-v /opt/portainer:/data portainer/portainer
```
-------------------------------------------------------------------------

# INSTALLATION:

Provided that the requirements listed above are met, please follow the instructions below:


## 1 - Clone the _"Neutrona Telemetry"_ repository and switch to the repository base directory:

```
$ git clone https://github.com/neutrona/neutrona_telemetry.git
$ cd neutrona_telemetry
```


## 2 - Configure the input plugin in `logstash_telemetry/logstash.conf` according to your profile (as provided by Neutrona):

For example: 
```
  $ nano logstash_telemetry/logstash
```

```shell
  user => "your_username"
  password => "your_password"
  queue => "neutrona_customers_telemetry_YOUR_CUSTOMER_NAME"
```

_All parameters are case sensitive._


## 3 - Docker compose and daemonize the app (this might take a few minutes):

```
  $ sudo docker-compose up -d
```


## 4 - Dashboard engine - Grafana:


### 4.1 - Point your browser to `http://your-ip-here:3000` and login.

	Default credentials: admin/admin


### 4.2 - Add a data source:
 
  * Click on _"Grafana Menu -> Data Sources -> Add Data Source"_ and set the parameters as below:

	- Name: _neutrona_telemetry_
	- Type: _InfluxDB_
	- URL: _http://influxdb:8086_
	- Database: _neutrona_telemetry_
	
  _You can leave the rest as default_


### 4.3 - Add your dashboard:

  * Navigate to _"Grafana Menu" -> "Dashboards" -> "Home"_
  * Click on _"Home Menu" -> "Import Dashboard" -> "Upload .json File"_
  * Select the file in `neutrona_telemetry/grafana/Grafana Dashboard.json` and click "Import"
	
  _At this point you should be able to see the data coming in from the telemetry broker :chart_with_upwards_trend:_



# Troubleshooting Tips:

- Please make sure you can reach the telemetry broker.
- Please check that the file `logstash_telemetry/logstash.conf` was modified as instructed and is syntactically correct.
- Grafana: 
    - Please check that the data source has been configured correctly under _"Grafana Menu -> Data Sources"_
    - If you encounter the _"Could not find the specified database name"_ error while adding the data source execute the following commands from the Docker host CLI:
    
	```
	$ sudo docker exec -it influxdb bash
	# influx
	> create database neutrona_telemetry
	> quit
	# exit
	```
	
## To rebuild the app:

```
$ sudo docker-compose down
$ sudo docker-compose build logstash_telemetry
$ sudo docker-compose up -d
```
