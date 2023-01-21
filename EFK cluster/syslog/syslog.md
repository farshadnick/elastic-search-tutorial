
# Fluentd Config 
```
<source>
  @type syslog
  port 5140
  @log_level debug
  bind 0.0.0.0
  tag rsyslog
</source>

<match rsyslog.** >
  @type elasticsearch
  host 192.168.4.31
  port 9200
  @log_level debug
  user nginx-edge
  password 82RPZ8FLFZsq6Cd
  index_name syslog
  logstash_format false
  scheme https
  ssl_verify false
</match>
```
