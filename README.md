# rsyslog configs
Given how long it took me to sort out Rsyslog config options, I decided to post my config in the hopes that it helps others.

Basically I had two needs -

1. Wrap syslog lines (messages, secure, etc.) in JSON format
2. Ship syslog and other local file based logs to a central Kafka message broker for processing by multiple entities
 
After going though evaluations of various log shippers (FluentD, Filebeats, others), I settled upon rsyslog as it was the de-facto installed program for my company's chosen OS - Centos.  One of the big reasons I chose Rsyslog is that it can use iNotify to know when to ship log lines as opposed to polling.

I currently ship my secure and messages files to a single Kafka topic, and I ship 4 different web logs into Kafka where they are consumed by various applications.  Alternately you can set up Rsyslog to send to a central "rsyslog" server should you choose to do so.

Using this config requires two packages from the rsyslog repos, and several dependencies:

  - libfastjson-0.99.2-1.el7.x86_64.rpm
  - libgt-0.3.11-1.el7.x86_64.rpm
  - liblogging-1.0.5-1.el7.x86_64.rpm
  - librdkafka1-0.8.5-0.x86_64.rpm
  - rsyslog-8.17.0-1.el7.x86_64.rpm
  - rsyslog-kafka-8.17.0-1.el7.x86_64.rpm
  
** Important note - if you run SELinux, you will need to execute one command to allow Rsyslog to talk to Kafka:**
```
semanage port -a -t syslogd_port_t -p tcp 9092
```

I've commented the config as best I can.
