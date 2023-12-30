# rivflux
Rivian R1T Data logger using InfluxDB, Telegraf, and Grafana.

The goal of this project is to be able to surface battery history, degradation tracking, location (if enabled), charging, and other metrics available.

This project is a WIP and is currently missing telegraf to be configured to poll. This piece is *required* and this will not work until avilable. Stay tuned, currently finishing up authentication to rivian in a more generic way.

This repo will also include a dashboard and telegraf config to track metrics for OpenEVSE.

### How to Use
Install `docker` and `docker-compose`.

Configure the fields in `docker-compose.yaml`
- openevse ip
- rivian vehicle id

`docker-compose up -d`

- Open grafana at `http://127.0.0.1:3000`
- Login using the credentials (`GF_SECURITY_ADMIN_USER` \ `GF_SECURITY_ADMIN_PASSWORD`) configured in `docker-compose.yaml`

### Example
<img width="1508" alt="0" src="https://github.com/rivflux/rivflux/assets/155249827/18f7d074-6a49-4b33-bc0b-bd5d4ea740e3">
<img width="1507" alt="1" src="https://github.com/rivflux/rivflux/assets/155249827/820999da-f84d-4dde-be8a-f86d5b391b2b">
<img width="1493" alt="2" src="https://github.com/rivflux/rivflux/assets/155249827/08e47ed7-657a-4a3a-91d0-7bd77c2b21ff">
