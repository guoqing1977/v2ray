archStudent
j1u5mko7


LM+DS+MC 10.0.3.7/13.145.153.62
CM                 10.0.3.8/13.148.245.35
IDX1               10.0.3.2/13.129.217.193
IDX2               10.0.3.3/13.147.8.3
IDX3               10.0.3.4/13.142.245.184
SH                  10.0.3.1/13.139.81.251







ssh 10.0.12.4 wget -O splunk-9.1.8-d45427bb0c27-Linux-x86_64.tgz "https://download.splunk.com/products/splunk/releases/9.1.8/linux/splunk-9.1.8-d45427bb0c27-Linux-x86_64.tgz"


ssh 10.0.12.5 wget -O splunkforwarder-9.1.8-d45427bb0c27-Linux-x86_64.tgz "https://download.splunk.com/products/universalforwarder/releases/9.1.8/linux/splunkforwarder-9.1.8-d45427bb0c27-Linux-x86_64.tgz"


/opt/splunk/bin/splunk enable web-ssl




/opt/splunk/bin/splunk edit cluster-config -mode manager -replication_factor 2 -search_factor 2 -secret j1u5mko7


/opt/splunk/bin/splunk set servername QING-IDX3
/opt/splunk/bin/splunk set default-hostname QING-IDX3



/opt/splunk/bin/splunk disable webserver

/opt/splunk/bin/splunk edit licenser-localslave -master_uri  https://10.0.3.7:8089

/opt/splunk/bin/splunk edit cluster-config -mode peer -manager_uri https://10.0.3.8:8089  -replication_port 9887 -secret j1u5mko7

/opt/splunk/bin/splunk enable listen 9997

/opt/splunk/bin//splunk btool check --debug


/opt/splunk/bin/splunk set servername QING-SH
/opt/splunk/bin/splunk set default-hostname QING-SH

/opt/splunk/bin/splunk edit cluster-config -mode searchhead  -manager_uri https://10.0.3.8:8089  -replication_port 9887 -secret j1u5mko7




/opt/splunk/bin/splunk  add  forward-server 10.0.3.2:9997

/opt/splunk/bin/splunk  add  forward-server 10.0.3.3:9997

/opt/splunk/bin/splunk  add  forward-server 10.0.3.4:9997



[indexAndForward]
index = false

[tcpout]
defaultGroup = default-autolb-group
forwardedindex.filter.disable = true
indexAndForward = false

[tcpout-server://10.0.3.2:9997]
[tcpout-server://10.0.3.3:9997]
[tcpout-server://10.0.3.4:9997]

[tcpout:default-autolb-group]
disabled = false
server = 10.0.3.2:9997,10.0.3.3:9997,10.0.3.4:9997







setfacl -R -m u:splunk:rx  /var/log


[monitor:///var/log]
disabled = 0
index=os

searhead install app --------props mapping


[monitor:///opt/log/www*/access.log]
index = network
host_segment = 3
disabled = false




[monitor:///opt/log/cisco_router1/cisco_ironport_web.log]
index = network
disabled = false
sourcetype = cisco:esa:http

[monitor:///opt/log/cisco_router1/cisco_ironport_mail.log]
index = network
disabled = false
sourcetype = cisco:esa:textmail


cm props.conf
[dreamcrusher]
SHOULD_LINEMERGE=false
LINE_BREAKER=([\r\n]+)\s*<Interceptor>
NO_BINARY_CHECK=true
TIME_FORMAT=%Y-%m-%d
TIME_PREFIX=<ActionDate>
TZ=GMT
MAX_TIMESTAMP_LOOKAHEAD=128





(?=[^M]*(?:MID|M.*MID))^(?:[^ \n]* ){8}(?P<mid>\d+)



<AttackVessel>(?<AttackVessel>[^<]+)</AttackVessel>



^[^/\n]*/\d+\s+\d+\s+\w+\s+(?P<url>[^ ]+)\s+(?P<user>[^ ]+)\s+\w+/(?P<domain>[^ ]+)










