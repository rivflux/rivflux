FROM registry.access.redhat.com/ubi9/ubi-minimal
RUN echo -e "\
[influxdb] \n\
name = InfluxData Repository - Stable \n\
baseurl = https://repos.influxdata.com/stable/\$basearch/main \n\
enabled = 1 \n\
gpgcheck = 1 \n\
gpgkey = https://repos.influxdata.com/influxdata-archive_compat.key\
" > /etc/yum.repos.d/influxdb.repo

RUN microdnf install -y python3 python3-pip go telegraf
COPY rivian /app
WORKDIR /app
RUN pip3 install -r /app/rivian-python-api/requirements.txt

EXPOSE 8125/udp 8092/udp 8094

ENTRYPOINT ["/app/entrypoint.sh"]
CMD ["telegraf"]
