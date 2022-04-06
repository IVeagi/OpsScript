input {
    kafka {
        codec => json
        group_id => "XXXXXb"
        auto_offset_reset => "earliest"
        topics  => [ "XXXXXX" ]
        #heartbeat_interval_ms => "15000"
        source => "message"
            temp_hash=event.get('log_hash')
            event.set('temp_hash',temp_hash)
            temp_hash.each{|k,v|
                event.set(k,v)
                    temp2_hash.delete(k)
                elsif v.class.to_s=='BigDecimal'
                    temp2_hash[k] = v.to_f
                end
            }
            event.set('reqtimestamp', event.get('logtime')*1000000)
            event.set('extra',temp2_hash.to_s.gsub('=>', ':'))
        "
    }
    grok {
        match => [ "path", '/home/log/log_collect/auto(?<dbname>\w+)/.*_(?<requestdate>\d+).log']
    }
    mutate {
        rename => { "type" => "tablename" }
        remove_field => [ "@timestamp", "path", "kafkatopic", "tags", "@version"]
        gsub => [ "extra", ":nil",":null"]
    }
#    if [dbname] == "baggift" or [dbname] == "buygiftstat" or [dbname] == "dbupdateevent" or [dbname] == "dspad" or [dbname] == "givegiftsync" or [dbname] == "pushdata" or [dbname] == "recommendshow" or [dbname] == "registermember" or [dbname] == "taskcenter" or [dbname] == "userworkupload" or [dbname] == "wuyuehua" or [dbname] == "membership" or [dbname] == "giftpackage"  or [dbname] == "userpop" or [dbname] == "toplist"  or [dbname] == "robtosing" or [dbname] == "activity"  or [dbname] == "newpersonranking" or [dbname] == "humsearchdata"  or [dbname] == "singtask" or [dbname] == "qudian" or [dbname] == "memberlevel" or [dbname] == "ktvroom" or [dbname] == "tasktab" or [dbname] == "doraemon" or [dbname] == "usersafty" or [dbname] ==  "abtest" or [dbname] ==  "payment"  or [dbname] == "wwwdata" or [dbname] == "imagedistinguish" or [dbname] ==  "noticedata" or [dbname] ==  "memberlevel"  or [dbname] == "changbamoment" or [dbname] == "memberactivity" or [dbname] == "richlevel" or [dbname]==  "onlinechorus" or [dbname] == "newuserrecharge" or [dbname] == "family" {

if [dbname] == "baggift" or [dbname] == "buygiftstat" or [dbname] == "dbupdateevent" or [dbname] == "dspad" or [dbname] == "givegiftsync" or [dbname] == "pushdata" or[dbname] == "recommendshow" or [dbname] == "registermember" or [dbname] == "taskcenter" or [dbname] == "userworkupload" or [dbname] == "wuyuehua" or [dbname] == "membership" or [dbname] == "giftpackage"  or [dbname] == "userpop" or [dbname] == "toplist"  or [dbname] == "robtosing" or [dbname] == "activity"  or [dbname] == "newpersonranking" or [dbname] == "humsearchdata"  or [dbname] == "singtask" or [dbname] == "qudian" or [dbname] == "memberlevel" or [dbname] == "ktvroom" or [dbname] == "tasktab" or [dbname] == "doraemon" or [dbname] == "usersafty" or [dbname] ==  "abtest" or [dbname] ==  "payment"  or [dbname] == "wwwdata" or [dbname] == "imagedistinguish"or [dbname] ==  "noticedata" or [dbname] ==  "memberlevel"  or [dbname] == "changbamoment" or [dbname] == "memberactivity" or [dbname] == "richlevel" or [dbname] ==  "onlinechorus" or [dbname] == "newuserrecharge" or [dbname] == "family" or [dbname] == "smsasync" {
        prune {
            whitelist_names => ["tablename","actype","curuserid","userid","workid","ip","macaddress","version","channelsrc","reqtimestamp","extra","requestdate","dbname","clienttype"]
        }
    } else { drop { } }

}
output {
    datahub {
        access_id => "LXXXO"
        access_key => "XXXXXXXXI"
        endpoint => "http://dh-cn-beijing-int-vpc.aliyuncs.com"
        project_name => "XXXX"
        retry_times => 5
        topic_name => "XXXXXX"
        dirty_data_continue => true
        dirty_data_file => "/home/logstash/logs/dirty_data.log"
        dirty_data_file_max_size => 10000
    }
#    stdout{ codec => rubydebug }
}
