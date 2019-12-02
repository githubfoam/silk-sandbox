# silk-sandbox

can not be ansibleable
~~~~
sudo systemctl enable rwflowpack
sudo systemctl start rwflowpack.service
sudo systemctl enable yaf
sudo systemctl start yaf.service

sudo systemctl restart rwflowpack.service
sudo systemctl status rwflowpack.service
sudo systemctl restart yaf.service
sudo systemctl status yaf.service
cat /var/log/rwflowpack-20191202.log

ping -c 5 8.8.8.8
/usr/local/bin/rwfilter --sensor=S0 --type=all --all=stdout | rwcut --tail-recs=10

<https://tools.netsa.cert.org/silk/silk-on-box-deb.html>
~~~~
smoketesting silk
~~~~
Generate traffic

$ ping -c 5 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=47 time=62.3 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=47 time=62.4 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=47 time=66.4 ms
64 bytes from 8.8.8.8: icmp_seq=4 ttl=47 time=61.6 ms
64 bytes from 8.8.8.8: icmp_seq=5 ttl=47 time=62.1 ms

$ sudo systemctl status yaf.service
● yaf.service - SYSV: Control yaf as a live capture daemon
   Loaded: loaded (/etc/init.d/yaf; generated)
   Active: active (running) since Mon 2019-12-02 10:17:46 UTC; 5min ago
     Docs: man:systemd-sysv-generator(8)
  Process: 10372 ExecStart=/etc/init.d/yaf start (code=exited, status=0/SUCCESS)
    Tasks: 1 (limit: 542)
   Memory: 1.8M
   CGroup: /system.slice/yaf.service
           └─10384 /usr/local/bin/yaf -d --live pcap --in eth1 --ipfix tcp --out localhost --ipfix-port 18001 --log /var/log/yaf.log --verbose --silk --applabel --max-payload=512 --pidfile /var/run/yaf.pid

Dec 02 10:17:44 vg-mokapot-03 systemd[1]: Starting SYSV: Control yaf as a live capture daemon...
Dec 02 10:17:46 vg-mokapot-03 yaf[10372]: Starting yaf:        [OK]
Dec 02 10:17:46 vg-mokapot-03 systemd[1]: Started SYSV: Control yaf as a live capture daemon.

$ sudo systemctl status rwflowpack.service
● rwflowpack.service - LSB: start and stop SiLK rwflowpack daemon
   Loaded: loaded (/etc/init.d/rwflowpack; generated)
   Active: active (running) since Mon 2019-12-02 10:15:24 UTC; 8min ago
     Docs: man:systemd-sysv-generator(8)
  Process: 9431 ExecStart=/etc/init.d/rwflowpack start (code=exited, status=0/SUCCESS)
    Tasks: 4 (limit: 542)
   Memory: 2.4M
   CGroup: /system.slice/rwflowpack.service
           └─9450 /usr/local/sbin/rwflowpack --sensor-configuration=/var/silk/sensors.conf --output-mode=local-storage --root-directory=/var/silk/data --pidfile=/var/run/rwflowpack.pid --log-level=info --log-dir

Dec 02 10:15:23 vg-mokapot-03 systemd[1]: Starting LSB: start and stop SiLK rwflowpack daemon...
Dec 02 10:15:24 vg-mokapot-03 rwflowpack[9431]: Starting rwflowpack:        [OK]
Dec 02 10:15:24 vg-mokapot-03 systemd[1]: Started LSB: start and stop SiLK rwflowpack daemon.


$ sudo systemctl status rwflowpack.service --no-pager
● rwflowpack.service - LSB: start and stop SiLK rwflowpack daemon
   Loaded: loaded (/etc/init.d/rwflowpack; generated)
   Active: active (running) since Mon 2019-12-02 10:15:24 UTC; 8min ago
     Docs: man:systemd-sysv-generator(8)
  Process: 9431 ExecStart=/etc/init.d/rwflowpack start (code=exited, status=0/SUCCESS)
    Tasks: 4 (limit: 542)
   Memory: 2.4M
   CGroup: /system.slice/rwflowpack.service
           └─9450 /usr/local/sbin/rwflowpack --sensor-configuration=/var/silk/sensors.conf --output-mode=local-storage --root-directory=/var/silk/data --pidfile=/var/run/rwflowpack.pid --log-level=info --log-di…

Dec 02 10:15:23 vg-mokapot-03 systemd[1]: Starting LSB: start and stop SiLK rwflowpack daemon...
Dec 02 10:15:24 vg-mokapot-03 rwflowpack[9431]: Starting rwflowpack:        [OK]
Dec 02 10:15:24 vg-mokapot-03 systemd[1]: Started LSB: start and stop SiLK rwflowpack daemon.


$ sudo systemctl status -l rwflowpack.service
● rwflowpack.service - LSB: start and stop SiLK rwflowpack daemon
   Loaded: loaded (/etc/init.d/rwflowpack; generated)
   Active: active (running) since Mon 2019-12-02 10:15:24 UTC; 9min ago
     Docs: man:systemd-sysv-generator(8)
  Process: 9431 ExecStart=/etc/init.d/rwflowpack start (code=exited, status=0/SUCCESS)
    Tasks: 4 (limit: 542)
   Memory: 2.4M
   CGroup: /system.slice/rwflowpack.service
           └─9450 /usr/local/sbin/rwflowpack --sensor-configuration=/var/silk/sensors.conf --output-mode=local-storage --root-directory=/var/silk/data --pidfile=/var/run/rwflowpack.pid --log-level=info --log-dir

Dec 02 10:15:23 vg-mokapot-03 systemd[1]: Starting LSB: start and stop SiLK rwflowpack daemon...
Dec 02 10:15:24 vg-mokapot-03 rwflowpack[9431]: Starting rwflowpack:        [OK]
Dec 02 10:15:24 vg-mokapot-03 systemd[1]: Started LSB: start and stop SiLK rwflowpack daemon

Run a test query
$ sudo /usr/local/bin/rwfilter --sensor=S0 --type=all --all=stdout | rwcut --tail-recs=10
                                    sIP|                                    dIP|sPort|dPort|pro|   packets|     bytes|   flags|                  sTime| duration|                  eTime|sen|
                           192.168.20.1|                        239.255.255.250|50222| 1900| 17|         4|       808|        |2019/12/02T10:13:44.092|    3.005|2019/12/02T10:13:47.097| S0|
                           192.168.20.1|                        239.255.255.250|57301| 1900| 17|         4|       808|        |2019/12/02T10:15:44.090|    3.004|2019/12/02T10:15:47.094| S0|
                           192.168.20.1|                        239.255.255.250|59544| 1900| 17|         1|       202|        |2019/12/02T10:17:44.133|    0.000|2019/12/02T10:17:44.133| S0|
                           192.168.20.1|                        239.255.255.250|59544| 1900| 17|         3|       606|        |2019/12/02T10:17:45.135|    2.003|2019/12/02T10:17:47.138| S0|
                           192.168.20.1|                         192.168.20.255|  137|  137| 17|         6|       468|        |2019/12/02T10:14:21.559|  102.643|2019/12/02T10:16:04.202| S0|

~~~
