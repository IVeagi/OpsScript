filebeat.inputs:
  - type: log
    enabled: true
    paths:
      - /home/log/log_collect/*/*
    fields:
      log_topic: log-collect

  - type: log
    enabled: true
    paths:
      - /home/log/recommend/access.log.*
    fields:
      log_topic: access

  - type: log
    enabled: true
    paths:
      - /home/log/recommend/log_resource/*.log.*
    fields:
      log_topic: log-resource

processors:
  - drop_fields:
      fields: ["agent", "log.offset", "input", "ecs"]
  
output.kafka:
  enabled: true
  hosts: ["1.1.1.1:9092", "2.2.2.2:9092","3.3.3.3:9092"]
  topic: '%{[fields.log_topic]}'
  partition.round_robin:
    reachable_only: false
    
#output.file:
 # path: "/tmp"
  #filename: test     

  required_acks: 1
  compression: gzip
  max_message_bytes: 1000000

logging.level: info
logging.to_files: true
logging.files:
  path: /var/log/filebeat
  name: filebeat
  rotateeverybytes: 1073741824
  keepfiles: 7
  permissions: 0606
  interval: 24h
 
