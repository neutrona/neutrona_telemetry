FROM docker.elastic.co/logstash/logstash:5.6.0
RUN logstash-plugin install logstash-output-influxdb
COPY logstash.conf /usr/share/logstash/pipeline/
CMD ["-f", "/usr/share/logstash/pipeline/"]
