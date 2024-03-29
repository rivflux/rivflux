# rivian - x logger (rivflux)
decentralized data logger for rivian vehicles, or at least the r1t. This project is not affiliated with Rivian and should be viewed as an unsupported third party integration and any changes on Rivian's side could break this app at anytime.

This uses the rivian app api, which seems to be okay in my experience but I am not sure Rivian's official stance on 3rd party tools like this one.  Please continue at your own risk.

I would consider this in an pre-aplha or proof of concept state, but things are stable and work well.  It uses three main tools: `influxdb, influxdb's telegraph, and grafana`


### Influx
Influxdb is used as a time series database to keep a record of each polling event


### Telegraf
Influxdb's telegraph is configured to poll Rivian's app api at an very slow rate. 5 minutes per poll.  This is configurable to log more accurately, but as a good tradeoff between historical logging/being nice to rivian's servers/not really needing extremely precise tracking I think this has been okay.


### Grafana
Grafana is used to visualize the data in influxdb


### Example Screenshot
<img width="1510" alt="0" src="https://github.com/rivflux/rivflux/assets/155249827/8e26413f-7e95-4726-a5bb-c280eeb36ac0">
<img width="1497" alt="1" src="https://github.com/rivflux/rivflux/assets/155249827/cfea97ed-038d-4872-8275-a836e5ba79fd">
<img width="1487" alt="2" src="https://github.com/rivflux/rivflux/assets/155249827/412949af-9a7d-44ac-9f7e-2d396d63b7b4">


### Unofficial Rivian API Docs Used
https://rivian-api.kaedenb.org/app/

This currently uses an off the shelf tool, slightly modified to poll the rivian API, and I am slowly working at making a custom suited one for this application.  But things seem to work well so far =]

https://github.com/the-mace/rivian-python-api

The modifications were necessary as it uses `rivian_cli.py`'s query command to output in the format that influxdb's telegraph can read and understand.  See the techincal details below to see the data being logged, as this is the def for the query command that telegraf is using.


#### Techincal Details
Telegraf is configured to poll using `rivian_cli.py` every five minutes. It is configured to log the data in a CSV file to the influx database. It uses `rivian_cli.py` which was changed to only output text that would be expected in a csv.  The header of the csv document, or the data logged, in it's current form is:

```python
        print(f"timestamp,Cabin Temp,Power,Drive Mode,Gear,Mileage,Battery,Range,{lat_long_title}Charger Status,Charge State,Battery
 Limit,Charge End,Charge Elapsed,Charge Power,Charge Session")
```

> Please refer to around line [1013](https://github.com/rivflux/rivflux/blob/8e6e1efe447d9c6b8f30a61d544646d3bdb67b54/telegraf/rivian/rivian-python-api/src/rivian_python_api/rivian_cli.py#L1013) to see the updated values in case they have changed.


#### New Rivian account for this
I suggest that you consider using a new rivian account for this tool. This is important in my view as it keeps your main rivian account seperated from the automated polling. This is important in case Rivian rate limits your polling or any other issue occurs.

The process to do this is:
- Create a new Rivian account
- Share your vehicle with the new Rivian account and use this new account in the steps below.


## Why was this made and what are some of the ideas?
I started off with a Tesla Model 3 and used the amazing TeslaMate since 2018.  I think that it's neat to be able to visualize the historical data logged and try to analize what is happening over time.  Having a way to track battery degregation on my own server is important to me, and it would also be nice to see some deep charging metrics.  Location is a questionable privacy topic, and I would like to integrate a way to easily disable logging this.

Open to changes, issues, and suggestions =]


## Install
Install `docker` and `docker-compose`.

Configure the fields in `docker-compose.yaml`
- openevse ip (will make this optional since not everyone has OpenEVSE, use 127.0.0.1 for now if you don't have one, telegraf will throw errors every few minutes but it's ok for now)
- rivian vehicle id (TODO - how to find this (after logging in/see below): `python3 rivian-python-api/src/rivian_python_api/rivian_cli.py --vehicles`)


Login to rivian's API and generate the pickle file to store the credentials to be able to use when polling.
### todo - how to do this and update docker-compose to make it easy to mount this secret
high level now (will clean this up)
- generate rivian_auth.pickle using instructions in https://github.com/the-mace/rivian-python-api#login

```
cd rivflux/telegraf/rivian
export RIVIAN_USERNAME="email@email.com"; export RIVIAN_PASSWORD="super_secure_password"; python3 rivian-python-api/src/rivian_python_api/rivian_cli.py --login

Login successful
```

- be sure the pickle file is saved in `telegraf/rivian/rivian_auth.pickle` (it should be from the previous command)
- run `docker-compose build`

Now start the stack and start logging!
`docker-compose up -d`

## Usage
Open grafana and view the Rivian dashboard

- Open grafana at `http://127.0.0.1:3000`
- Login using the credentials (`GF_SECURITY_ADMIN_USER` \ `GF_SECURITY_ADMIN_PASSWORD`) configured in `docker-compose.yaml`


## Backup
todo...some form of backup based on influx's documentation, maybe also compress for archival

## Restore
todo...how to restore completely in the event of failure and you have a backup.  High level, maybe run Install again and then folow influx's restore procedures.

