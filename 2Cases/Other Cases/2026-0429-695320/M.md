2026-0429-695320 - 20618 - SINGTEL -- MEGAPOP - NERA - MX-BP-02-06 - [Singtel:Singtel-Megapop:MX-BP-02-06] Health Check- FPC 3 | Ticket #20618




---------
Hi Naveen,

1./ This case do not require handing over to the next engineer.

2./ Summary of troubleshooting done to recover service.


Singtel reported service impact at the following interfaces at approximately 1230PM 29 April SGT. (ge-3/0/1, ge-3/0/3, ge-3/0/4,ge-3/1/4,ge-3/1/6,ge-3/1/7).
Base on traffic monitoring graph, no traffic since 4pm 28 April SGT (Graph uploaded to JSP)

Base State before any troubleshooting

ge-3/0/0                up    up
ge-3/0/0.0              up    up   inet     172.16.24.189/30
ge-3/0/1                up    up
ge-3/0/1.0              up    up   inet     172.16.20.45/30
ge-3/0/2                up    up
ge-3/0/2.0              up    up   inet     198.18.85.101/30
ge-3/0/3                up    up
ge-3/0/3.0              up    up   inet     172.16.20.65/30
ge-3/0/4                up    up
ge-3/0/4.0              up    up   inet     172.16.20.177/30
ge-3/0/5                up    up
ge-3/0/5.0              up    up   inet     172.16.1.113/30
ge-3/0/6                up    down
ge-3/0/7                up    up
ge-3/0/7.0              up    up   inet     192.169.3.254/24
ge-3/0/8                up    down
ge-3/0/8.0              up    down inet     192.169.4.254/24
ge-3/0/9                up    up
ge-3/0/9.0              up    up   inet     192.169.14.254/24
ge-3/1/0                up    up
ge-3/1/0.0              up    up   inet     192.169.10.254/24
ge-3/1/1                up    up
ge-3/1/1.0              up    up   inet     192.169.9.254/24
ge-3/1/2                up    up
ge-3/1/2.0              up    up   inet     192.169.13.254/24
ge-3/1/3                up    up
ge-3/1/3.0              up    up   inet     192.169.7.254/24
ge-3/1/4                up    up
ge-3/1/4.0              up    up   inet     172.16.1.33/30  
ge-3/1/5                up    down
ge-3/1/5.0              up    down ccc    
ge-3/1/6                up    up
ge-3/1/6.0              up    up   inet     172.16.20.53/30
ge-3/1/7                up    up
ge-3/1/7.0              up    up   inet     172.16.20.37/30
ge-3/1/8                up    down
ge-3/1/8.16386          up    down
ge-3/1/9                up    down
ge-3/1/9.16386          up    down

We used shell command "start shell pfe network fpc3 " "set parser security 10" "show sfp list "
We reinitialized ge-3/1/7, ge-3/1/6,ge-3/0/4. Not able to recover traffic.

NGMPC3(MX-BP-02-06 vty)# test sfp 54 reinitialize

NGMPC3(MX-BP-02-06 vty)# [Apr 29 05:53:20.316 LOG: Notice] MIC(3/1): Link 7 SXFX Phy based SFP  - plugged in.

NGMPC3(MX-BP-02-06 vty)# test sfp 53 reinitialize

NGMPC3(MX-BP-02-06 vty)# [Apr 29 05:54:49.467 LOG: Notice] MIC(3/1): Link 6 SXFX Phy based SFP  - plugged in.

NGMPC3(MX-BP-02-06 vty)# test sfp 41 reinitialize

NGMPC3(MX-BP-02-06 vty)# [Apr 29 05:56:42.098 LOG: Notice] MIC(3/0): Link 4 SXFX Phy based SFP  - plugged in.


We reboot MIC 0 once Singtel gotten the clearence to perform the reboot.

P1256869_RW@MX-BP-02-06> request chassis mic offline fpc-slot 3 mic-slot 0    
fpc 3 mic 0 offline initiated, use "show chassis fpc pic-status 3" to verify

We noticed there are more interface that has up down state.

ge-3/0/0        up    down CUST~MP~LTA~Singapore~00201350SNG~HS-EL~2M~LTA-1886~~NISA
ge-3/0/0.0      up    down CUST~MP~LTA~Singapore~00201350SNG~HS-EL~2M~LTA-1886~~NISA
ge-3/0/1        up    up   CUST~MP~LTA~~00059134SNG~HS-EL~2M~LTA-1886~~TINA
ge-3/0/1.0      up    up   CUST~MP~LTA~~00059134SNG~HS-EL~2M~LTA-1886~~TINA
ge-3/0/2        up    down CUST~MP~MOD~~00098007SNG~HS-EL~20M~MOD-19829~~TINA
ge-3/0/2.0      up    down CUST~MP~MOD~~00098007SNG~HS-EL~20M~MOD-19829~~TINA
ge-3/0/3        up    up   CUST~MP~LTA~Singapore~00059177SNG~HS-EL~2M~LTA-1886~~NISA
ge-3/0/3.0      up    up   CUST~MP~LTA~Singapore~00059177SNG~HS-EL~2M~LTA-1886~~NISA
ge-3/0/4        up    down CUST~MP~LTA~~00063975SNG~HS-EL~4M~LTA-1886~~TINA
ge-3/0/4.0      up    down CUST~MP~LTA~~00063975SNG~HS-EL~4M~LTA-1886~~TINA
ge-3/0/5        up    up   CUST~MP~LTA~~00235051SNG~HS-EL~2M~LTA-1886~~TINA
ge-3/0/5.0      up    up   CUST~MP~LTA~~00235051SNG~HS-EL~2M~LTA-1886~~TINA
ge-3/0/7        up    up   CUST~MP~POWERGAS-2~Singapore~00021879SNG~HS-EL~20M~POWERGAS-2-2861~~NISA
ge-3/0/7.0      up    up   CUST~MP~POWERGAS-2~Singapore~00021879SNG~HS-EL~20M~POWERGAS-2-2861~~NISA
ge-3/0/8        up    down CUST~MP~POWERGAS-3~Singapore~00021880SNG~HS-EL~20M~POWERGAS-3-2861~~NISA
ge-3/0/8.0      up    down CUST~MP~POWERGAS-3~Singapore~00021880SNG~HS-EL~20M~POWERGAS-3-2861~~NISA
ge-3/0/9        up    up   CUST~MP~POWERGAS-4~Singapore~00021889SNG~HS-EL~20M~POWERGAS-4-2861~~NISA
ge-3/0/9.0      up    up   CUST~MP~POWERGAS-4~Singapore~00021889SNG~HS-EL~20M~POWERGAS-4-2861~~NISA
ge-3/1/0        up    up   CUST~MP~POWERGAS-5~Singapore~00021875SNG~HS-EL~2M~POWERGAS-5-2861~~NISA
ge-3/1/0.0      up    up   CUST~MP~POWERGAS-5~Singapore~00021875SNG~HS-EL~2M~POWERGAS-5-2861~~NISA
ge-3/1/1        up    up   CUST~MP~POWERGAS-6~Singapore~00021888SNG~HS-EL~20M~POWERGAS-6-2861~~NISA
ge-3/1/1.0      up    up   CUST~MP~POWERGAS-6~Singapore~00021888SNG~HS-EL~20M~POWERGAS-6-2861~~NISA
ge-3/1/2        up    down CUST~MP~POWERGAS-7~Singapore~00021885SNG~HS-EL~20M~POWERGAS-7-2861~~NISA
ge-3/1/2.0      up    down CUST~MP~POWERGAS-7~Singapore~00021885SNG~HS-EL~20M~POWERGAS-7-2861~~NISA
ge-3/1/3        up    up   CUST~MP~POWERGAS-8~Singapore~00021883SNG~HS-EL~20M~POWERGAS-8-2861~~NISA
ge-3/1/3.0      up    up   CUST~MP~POWERGAS-8~Singapore~00021883SNG~HS-EL~20M~POWERGAS-8-2861~~NISA
ge-3/1/4        up    up   CUST~MP~LTA~Singapore~00053482SNG~HS-EL~2M~LTA-1886~~NISA
ge-3/1/4.0      up    up   CUST~MP~LTA~Singapore~00053482SNG~HS-EL~2M~LTA-1886~~NISA
ge-3/1/6        up    down CUST~MP~LTA~Singapore~00059136SNG~HS-EL~20M~LTA-1886~~NISA
ge-3/1/6.0      up    down CUST~MP~LTA~Singapore~00059136SNG~HS-EL~20M~LTA-1886~~NISA
ge-3/1/7        up    up   CUST~MP~LTA~Singapore~00059132SNG~HS-EL~2M~LTA-1886~~NISA
ge-3/1/7.0      up    up   CUST~MP~LTA~Singapore~00059132SNG~HS-EL~2M~LTA-1886~~NISA

We reinitialized ge-3/0/0, ge-3/0/3, ge-3/0/6, ge-3/1/6, ge-3/1/0, ge-3/0/7, ge-3/0/8, ge-3/1/3, ge-3/1/2, ge-3/0/2 and able to recover traffic and the interface to up up state.


Final State after all service is recovered

{master}
P1256869_RW@MX-BP-02-06> show interfaces terse | match ge-3
ge-3/0/0                up    up
ge-3/0/0.0              up    up   inet     172.16.24.189/30
ge-3/0/1                up    up
ge-3/0/1.0              up    up   inet     172.16.20.45/30
ge-3/0/2                up    up
ge-3/0/2.0              up    up   inet     198.18.85.101/30
ge-3/0/3                up    up
ge-3/0/3.0              up    up   inet     172.16.20.65/30
ge-3/0/4                up    up
ge-3/0/4.0              up    up   inet     172.16.20.177/30
ge-3/0/5                up    up
ge-3/0/5.0              up    up   inet     172.16.1.113/30
ge-3/0/6                up    down
ge-3/0/7                up    up
ge-3/0/7.0              up    up   inet     192.169.3.254/24
ge-3/0/8                up    down
ge-3/0/8.0              up    down inet     192.169.4.254/24
ge-3/0/9                up    up
ge-3/0/9.0              up    up   inet     192.169.14.254/24
ge-3/1/0                up    up
ge-3/1/0.0              up    up   inet     192.169.10.254/24
ge-3/1/1                up    up
ge-3/1/1.0              up    up   inet     192.169.9.254/24
ge-3/1/2                up    up
ge-3/1/2.0              up    up   inet     192.169.13.254/24
ge-3/1/3                up    up
ge-3/1/3.0              up    up   inet     192.169.7.254/24
ge-3/1/4                up    up
ge-3/1/4.0              up    up   inet     172.16.1.33/30  
ge-3/1/5                up    down
ge-3/1/5.0              up    down ccc    
ge-3/1/6                up    up
ge-3/1/6.0              up    up   inet     172.16.20.53/30
ge-3/1/7                up    up
ge-3/1/7.0              up    up   inet     172.16.20.37/30
ge-3/1/8                up    down
ge-3/1/8.16386          up    down
ge-3/1/9                up    down
ge-3/1/9.16386          up    down

3./ Session logs and traffic monitoring graph has been uploaded to JSP.

4./ Please assist to look into this case. 