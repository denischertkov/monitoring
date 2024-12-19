# compose-prometheus

## This is a docker-compose stack created for a network enviroment to monitor the network switches. It consits of a working SNMP exporter with Prometheus and Grafana. The node exporter also supporthed.

# Pre-requisites

1. Ensure you have [docker](https://docs.docker.com/engine/install/ubuntu/) and [docker compose](https://docs.docker.com/compose/install/) on your system. The simplest way to install the latest docker from the root user:
```
curl -sSL https://get.docker.com | sh
```
If you want to allow run docker to the user, grant the permessions to this user:
```
usermod -aG docker $USER
```

2. Create the folders:
```
mkdir -p /opt/monitoring/{prometheus,grafana,alertmanager,blackbox}
cd /opt/monitoring
```
4. Copy configuration files from the repo:

Copy the docker-compose.yml file from this repo to /opt/monitoring/docker-compose.yml

Copy the prometheus.yml file from this repo to /opt/monitoring/prometheus/prometheus.yml

5. In */opt/monitoring/prometheus/prometheus.yml*, add the IP addresses of the target switches to the target section after string number 15.
```
  - job_name: 'snmp'
    scrape_interval: 30s
    static_configs:
    - targets:
#	DC1
      - [NEW switch1 IP]       # main switch RU1-2
#	DC2
      - [NEW switch2 IP]       # second switch RU12
```
6. Need to enable the SNMP support on the Juniper switches:
```
set snmp community public
```
If you are using filtered access to the management interface, you must also explicitly enable SNMP access from the SNMP Exporter host:
```
set firewall family inet filter MGMT_ACCESS term SNMP from source-address 10.10.1.12/32
set firewall family inet filter MGMT_ACCESS term SNMP from protocol udp
set firewall family inet filter MGMT_ACCESS term SNMP from destination-port 161
set firewall family inet filter MGMT_ACCESS term SNMP then count
set firewall family inet filter MGMT_ACCESS term SNMP then accept
```

# Running

1. Go to the /opt/moniotoring directory
```
cd /opt/monitoring
```
2. Run the docker-compose stack with `docker compose up -d`

3. Open in the Web browser http://[docker node IP]:3000 and login with admin/admin

# Please note

1. Prometheus is running on port 9990/targets
2. Grafana is running on port 3000
3. SNMP Exporter is running on port 9116
4. Node Exporter is running on port 9100

Denis Chertkov, denis@chertkov.info, 20241218
