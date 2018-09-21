FROM docker.elastic.co/logstash/logstash:6.4.1
RUN logstash-plugin install logstash-output-influxdb
COPY logstash.conf /usr/share/logstash/pipeline/
CMD ["-f", "/usr/share/logstash/pipeline/"]
