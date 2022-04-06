input {
    kafka {
        codec => json
        group_id => "ops2es"
        auto_offset_reset => "earliest"
        topics  => [ "debug-collect" ]
        bootstrap_servers => "1.1.1.1:9092, 2.2.2.2:9092, 3.3.3.3:9092"
    }
}
filter {

    json {
input {
    kafka {
        codec => json
        group_id => "ops2es"
        auto_offset_reset => "earliest"
        topics  => [ "debug-collect" ]
        bootstrap_servers => "10.100.201.69:9092, 10.100.201.70:9092, 10.100.201.71:9092"
    }
}
filter {

    json {
        source => "message"
    }
    ruby {
        code => "
            path=event.get('log')['file']['path']
            event.set('path',path)
            host=event.get('host')['name']
            event.set('host',host)
            event.set('response_str',event.get('response').to_s.gsub('=>', ':'))
        "
    }
    date {
        match => ["logtime", "UNIX"]
        timezone => "Asia/Shanghai"
        target => "@timestamp"
    }

    grok {
        match => [ "path", '/home/log/debug_collect/(?<dbname>\w+)/.*_(?<requestdate>\d+).log']
    }
    mutate {
        rename => { "type" => "tablename" }
#        remove_field => [ "@timestamp", "path", "kafkatopic", "tags", "@version"]
        remove_field => [ "message","fields","log","@version"]
    }
    prune {
            whitelist_names => ["dbname","ac" ,"host" ,"retMsg","curuserid","@timestamp","response_str","ac",   "request","tablename","logtime"]
        }

}
output {
#    stdout{ codec => rubydebug }
    elasticsearch {
        hosts => ["XXXXXyuncs.com:9200"]
        index => "debuglog-%{dbname}-%{+YYYY.MM.dd}"
        user => "XXXXX"
        password => "*XXXX"
    }

}
