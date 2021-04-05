# mtr2grafana

A docker-compose file + sample configs for MTR receiver data visualization in Grafana. Note that the setup won't work purely on macOS since the usb serial port mapping to docker containers is basically undoable. 

Services used:
- [mtr2mqtt](https://github.com/tvallas/mtr2mqtt/), for transferring and enriching data from Nokeval MTR series transmitters to MQTT
- [Mosquitto](https://mosquitto.org/), MQTT Broker
- [Telegraf](https://www.influxdata.com/time-series-platform/telegraf/), to read the measurement data and pass it to ..
- [InfluxDB](https://www.influxdata.com/products/influxdb/), time-series database
- [Grafana](https://grafana.com/), time-series visualization tool 

All running in Docker without stuff installed in the host

## Contents

- `docker-compose.yml`, Docker compose file for defining the system 
- `influxdb.evn.template`, template for defining the DB and USER&PASS of Influxdb
- `telegraf.conf`, telegraf configuration file for transmitting measurements from MQTT to InfluxDB
- `metadata.yml`, sample metadata file for mtr2mqtt

## Usage
1. Add metadata (location, unit, quantity, description) for transmitters in metadata.yml

2. Set a user and password for influxdb 
```bash
cp influxdb.env.template influxdb.env
```
and just add username and password in the `influxdb.env`-file

3. Get data flowing

```bash
docker-compose up 
```
Check what is happening in the communication

4. Point your web browser to Grafana @ http://localhost:3000 and add Influxdb (V1) as Grafana source with `http://influxdb:8086` as the URL and `telegraf` as the DB name

5. Create some nifty dashboards

![grafana_dashboard](img/grafana.png)
   
### Running as a service
If you want to run the service in the background use 
```bash
docker-compose up -d
```
to run the Docker compose as a daemon. With `restart: always` set on Docker compose -file, the system should spring up a boot

