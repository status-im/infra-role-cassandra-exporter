# Description

This role configures the [Cassandra Exporter](https://github.com/criteo/cassandra_exporter) service which communicates with the [JMX](https://en.wikipedia.org/wiki/Java_Management_Extensions) metrics export port of Cassandra and exposes available metrics via Prometheus compatible endpoint.

# Configuration

The bare minimum would be:
```yml
# Cassandra JMS metrics port
cassandra_exp_jmx_addr: 'localhost'
cassandra_exp_jmx_port: 7199
# Local listening port
cassandra_exp_listen_addr: '0.0.0.0'
cassandra_exp_listen_port: 8080
```
The configuration file is created at `/etc/cassandra-exporter/config.yml`.

# Management

Service is managed with Systemd:
```
admin@store-03.do-ams3.metrics.hq:~ % sudo systemctl status cassandra-exporter
● cassandra-exporter.service - "Cassandra Exporter - Provides JMS metrics via a Prometheus-type endpoint"
     Loaded: loaded (/lib/systemd/system/cassandra-exporter.service; enabled; vendor preset: enabled)
     Active: active (running) since Tue 2020-08-11 11:31:23 UTC; 2min 32s ago
       Docs: https://github.com/criteo/cassandra_exporter
   Main PID: 801110 (java)
      Tasks: 26 (limit: 9513)
     Memory: 118.5M
     CGroup: /system.slice/cassandra-exporter.service
             └─801110 /usr/bin/java -jar /opt/cassandra-exporter/cassandra_exporter.jar /etc/cassandra-exporter/config.yml

Aug 11 11:31:23 store-03.do-ams3.metrics.hq systemd[1]: Started "Cassandra Exporter - Provides JMS metrics via a Prometheus-type endpoint".
Aug 11 11:31:24 store-03.do-ams3.metrics.hq java[801110]: [main] INFO com.criteo.nosql.cassandra.exporter.Config - Loading yaml config from /etc/cassandra-exporter/c>
Aug 11 11:31:26 store-03.do-ams3.metrics.hq java[801110]: [main] INFO com.criteo.nosql.cassandra.exporter.JmxScraper - Scrap took 2118ms for the whole run
Aug 11 11:32:15 store-03.do-ams3.metrics.hq java[801110]: [main] INFO com.criteo.nosql.cassandra.exporter.JmxScraper - Scrap took 880ms for the whole run
Aug 11 11:33:05 store-03.do-ams3.metrics.hq java[801110]: [main] INFO com.criteo.nosql.cassandra.exporter.JmxScraper - Scrap took 891ms for the whole run
Aug 11 11:33:55 store-03.do-ams3.metrics.hq java[801110]: [main] INFO com.criteo.nosql.cassandra.exporter.JmxScraper - Scrap took 680ms for the whole run
```

# Known Issues

*  The configuration created from [`templates/config.yml.j2`](./templates/config.yml.j2) limits exposed metrics
  - You can expand the available metrics by editing the `blacklist` attribute in the config.
