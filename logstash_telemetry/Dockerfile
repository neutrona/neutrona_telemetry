FROM docker.elastic.co/logstash/logstash:6.4.1
COPY logstash-offline-plugins-6.4.1.zip /tmp/
RUN logstash-plugin install --local --no-verify file:///tmp/logstash-offline-plugins-6.4.1.zip
COPY logstash.conf /usr/share/logstash/pipeline/
CMD ["-f", "/usr/share/logstash/pipeline/"]
