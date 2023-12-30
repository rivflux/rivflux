# rivflux
Rivian R1T Data logger using InfluxDB, Telegraf, and Grafana.

The goal of this project is to be able to surface battery history, degradation tracking, location (if enabled), charging, and other metrics available.

This project is a WIP and is currently missing telegraf to be configured to poll. This piece is *required* and this will not work until avilable. Stay tuned, currently finishing up authentication to rivian in a more generic way.

This repo will also include a dashboard and telegraf config to track metrics for OpenEVSE.

### How to Use
Configure the fields in `docker-compose.yaml`
- openevse ip
- rivian vehicle id

`docker-compose up -d`

- Open grafana at `http://127.0.0.1:3000`
- Login using the credentials (`GF_SECURITY_ADMIN_USER` \ `GF_SECURITY_ADMIN_PASSWORD`) configured in `docker-compose.yaml`