# LDP

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

LDP
LDP:
- udp 646， tcp 646， lo0 router-id
- ldp follow igp
- mpls.0 , inet.3
inet3里只放ingress lsp的.../32...路由，只有ingress router需要查看这个表。transit egress只要看mpls0表就知道怎么走。
- ldp database:
是label的database。input指的是remote routerr给local分的label，就是local要到egress router，走到nh带着的label。output指的是local给remote router分的label，就是当remote router要到local的时候，到nh要带着的label。
--FEC:A group of prefixes that has similar forwarding actions. LDP associated with FEC.
- ldp egress policy:可以把一个subnet或自己的lo0作为ldp路由放进inet3，否则就只有peer lo0的ldp路由
#ldp database
lab@R1# run show ldp database
Input label database, 172.30.5.1:0--172.30.5.2:0
Label     Prefix
299792     172.30.5.1/32
0           172.30.5.2/32
299776     172.30.5.3/32
299808     172.30.5.4/32
<<<local ...是r...1...，input是r2给r...1...分的label，就是r1到r2路由要带的label。而到r3就是，r1经过r...2...到r3时，r1要带着的label到达r...2
Output label database, 172.30.5.1:0--172.30.5.2:0
Label     Prefix
0           172.30.5.1/32
299872     172.30.5.2/32
299888     172.30.5.3/32
299904     172.30.5.4/32
<<<...local... ...r1... ...给r...2...分的label，就是r...2...要到r...1...时需要带的label。...R...3就是从r3来的路由经过r2时，r2要给他什么label让他走向r1....
<<<<...所以因为...explicit null...是最后一跳...pop...，...R1->R2...就是...R2...是最后一跳，他的...label...是...0 pop...。而...R2->R1...就是...R1...是最后一跳，...R1 ...是...0 pop
# inet.3
All LSP routes are installed in this table on ingress node.
lab@Sun# run show route table inet.3
inet.3: 3 destinations, 3 routes (3 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both
172.30.5.2/32      *[LDP/9] 00:43:13, metric 5
> to 172.30.0.2 via ae0.0, Push 0
172.30.5.3/32      *[LDP/9] 00:43:13, metric 15
> to 172.30.0.2 via ae0.0, Push 299776
172.30.5.4/32      *[LDP/9] 00:44:01, metric 10
> to 172.30.0.6 via ge-0/0/4.114, Push 0
》》》》从...R1...分别到这些...routers...，...nh...是什么，在...R1...上会被...push...一个什么...label...到...nh router...。比如，到...R4 nh...是...R4...的ge-0/0/4.114，带着...label 0...（就是说明...R4...是目的地...label0...在他上面...pop...。）
》》》》...inet.3...只存放...ingress ldp session...在...local router...上始发的。
》》》》... inet3...里只有到...egress router...的.../32...路由！！... ...只有...ingress router...需要看...inet3...决定走那条...ldp session...，... transit & egress router...看...mpls.0...知道...label...就可以了。
>>>default...，只有.../32...路由会被写进这个表。可以用...install knob...把非.../32...的路由加进来
--------------------------------------
lab@R3# run show ldp database
Input label database, 172.30.5.3:0--172.30.5.2:0
Label     Prefix
299792     172.30.5.1/32
0     172.30.5.2/32
299776     172.30.5.3/32
299808     172.30.5.4/32
Input label database, 172.30.5.3:0--172.30.5.4:0
Label     Prefix
299792     172.30.5.1/32
299808     172.30.5.2/32
299776     172.30.5.3/32
0     172.30.5.4/32
[edit]
lab@R3# run show route table inet.3
172.30.5.1/32      *[LDP/9] 00:33:04, metric 15
> to 172.30.0.13 via ge-0/0/4.123, Push 299792
《《《《...R1...出来无论走那个...session...都带着299792
172.30.5.2/32      *[LDP/9] 00:33:04, metric 10
> to 172.30.0.13 via ge-0/0/4.123, Push 0
《《《从...R1...始发到...R2 egress...的...ldpsession...需要带...0
172.30.5.4/32      *[LDP/9] 00:31:18, metric 10
> to 172.30.0.22 via ge-0/0/4.134, Push 0
《《《从...R1...始发到...R4 egress...的...ldpsession...需要带...0
-----------------------------
# mpls.0
<<<...包含所有在本台...router...上有...label...行为的...labels...，有...ingress transit egress
lab@Sun# run show route table mpls.0
mpls.0: 9 destinations, 9 routes (9 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both
0                  *[MPLS/0] 02:01:51, metric 1
Receive
1                  *[MPLS/0] 02:01:51, metric 1
Receive
2                  *[MPLS/0] 02:01:51, metric 1
Receive
13                 *[MPLS/0] 02:01:51, metric 1
Receive
299872             *[LDP/9] 00:32:20, metric 1
> to 172.30.0.6 via ge-0/0/4.114, Swap 0
299872(S=0)        *[LDP/9] 00:32:20, metric 1
> to 172.30.0.6 via ge-0/0/4.114, Pop
299888             *[LDP/9] 00:32:38, metric 1
> to 172.30.0.2 via ae0.0, Swap 0
299888(S=0)        *[LDP/9] 00:32:38, metric 1
> to 172.30.0.2 via ae0.0, Pop
299904             *[LDP/9] 00:32:38, metric 1
> to 172.30.0.2 via ae0.0, Swap 299872
>>>R1 ...上... show R1...如何分...label
lab@R1# run show ldp database
Input label database, 172.30.5.1:0--172.30.5.2:0...《
Label     Prefix
299776     172.30.5.1/32
0     172.30.5.2/32...《《《...label R1...到...R2
299792     172.30.5.3/32 ...《《...label R1...到...R3
299808     172.30.5.4/32
《《《下游给...R1...的...label...，为了到下游的...output route
《《...label input direction...，... route output direction
Output label database, 172.30.5.1:0--172.30.5.2:0
Label     Prefix
0     172.30.5.1/32
299776     172.30.5.2/32
299792     172.30.5.3/32
299808     172.30.5.4/32
《《《...R1...作为上游给下游的...label...，为了下游可以到自己
《《...label output direction...，... route input direction
在...R1...上...show inet3
lab@R1# run show route table inet.3 《《《...R1 route output...，看...R1...的...input label
172.30.5.2/32      *[LDP/9] 01:44:06, metric 5
> to 172.30.0.2 via ae0.0, Push 0...
172.30.5.3/32      *[LDP/9] 00:51:48, metric 15
> to 172.30.0.2 via ae0.0, Push 299792
lab@R2# run show route table inet.3
172.30.5.1/32      *[LDP/9] 02:38:58, metric 5《《路由从...R2...到...R1...是...input...看...R1...的...output label
> to 172.30.0.1 via ae1.0, Push 0
lab@R3# run show route table inet.3
172.30.5.1/32      *[LDP/9] 02:07:04, metric 15
> to 172.30.0.13 via ge-0/0/4.123, Push 299776
!!!...此处...R3...到...R1...的...label...并不符合...R1 output label...，因为中间隔着...R2...，...label...永远是为上下游或上游直连分配的。比如到...R2...和...R3...从...R1,...都要经过...R2,...但分的...label...却不一样，因为...destination ...不一样。
Input label database, 172.30.5.1:0--172.30.5.2:0...《
Label     Prefix
299776     172.30.5.1/32
0     172.30.5.2/32...《《《...R1...到...R2 ...带这个
299792     172.30.5.3/32 ...《《...R1...经过...R2...到...R3...带这个
3) ISIS -LDP Sync
Ensure LDP must be fully established before ISIS path are used for
--support only for P2P interfaces
lab@Sun# set protocols isis interface ge-0/0/4.114 ldp-synchronization
lab@Sun# set protocols isis interface ge-0/0/4.114 ...point-to-point
<<<<point-to-point ...一定不能忘，否则和其他接口类型不一样，...isis...起不来。
> ldp-synchronization  Advertise maximum metric until LDP is operational
lab@Sun# run show isis interface ae0.0 extensive
IS-IS interface database:
ae0.0
Index: 69, State: 0x6, Circuit id: 0x1, Circuit type: 1
LSP interval: 100 ms, CSNP interval: 20 s, Loose Hello padding
Adjacency advertisement: Advertise
LDP sync state: in sync, for: 00:00:55, reason: LDP up during config
config holdtime: infinity
Level 1
Adjacencies: 1, Priority: 64, Metric: 5
Hello Interval: 9.000 s, Hold Time: 27 s
IPV6 UnicastMetric: 5
4) ...把...ix facing...的...subnet...引入进...ldp
LDP egress policy <<< only on R1 R2!!
--by default, LDP only announce /32 loopback address.
--------------------------
lab@Arcturus# run show route protocol ldp
inet.0: 673 destinations, 697 routes (673 active, 17 holddown, 0 hidden)
inet.3: 2 destinations, 2 routes (2 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both
172.30.5.1/32      *[LDP/9] 00:00:10, metric 3
> to 172.30.0.5 via ge-0/0/4.114, Push 0
172.30.5.2/32      *[LDP/9] 00:00:10, metric 3
> to 172.30.0.5 via ge-0/0/4.114, Push 332512
172.30.5.3/32      *[LDP/9] 00:00:11, metric 3
> to 172.30.0.21 via ge-0/0/4.134, Push 0
《《《《...ldp session up...后，...inet0...里...ldp...路由没有，...mpls...都只让...ldp peer router...的.../32...进入...inet3...，而这些进来的路由也只是他们...lo0...（...router id...）的，虽然...lo0...并没有开启...ldp...，但是...router id...用来通告路由成为...ldp ...路由，...physical address...不会成为...ldp ...路由。
《《《并且本地...lo0...不会作为...ldp...路由出现在...inet3...里的，因为...lo0...并没有开启...ldp...，他只会被... ...通告给... peer ldp router...，在...peer router...上成为...ldp...路由。
--------------------------
但是题中要求把...192.168.1.0/24...这个... subnet...注入...ldp...。这需要让这个...subnet...也有...label...，写进...ldp database
《《《作用就是不配...ldp...的情况下让某些...routes...进...ldp...。...Egress policy...可以让某些...route...有...label...，而...export policy...只是把某些路由哦引进...protocol...里面！！有...label...就可以为到...IX...不同...router...的路由走不同...lsp...。
《《《之所以...egress policy...可以让...ix subnets...有不同...label...，是因为...ix...放进来成为...inet3...路由，那么...traffic...就是从其他...router...到...r12...，所以...egress policy...可以决定让包带着...label...过来。
------------------
lab@Sun# run show route protocol ldp
inet.0: 833 destinations, 1416 routes (671 active, 0 holddown, 324 hidden)
inet.3: 3 destinations, 3 routes (3 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both
172.30.5.2/32      *[LDP/9] 00:01:52, metric 5
> to 172.30.0.2 via ae0.0, Push 0
172.30.5.3/32      *[LDP/9] 00:00:09, metric 15
> to 172.30.0.2 via ae0.0, Push 303632
172.30.5.4/32      *[LDP/9] 00:02:38, metric 10
> to 172.30.0.6 via ge-0/0/4.114, Push 0
---------------------------
lab@Sun# set policy-options policy-statement ldp-routes term 1 from route-filter 192.168.1.0/24 exact
lab@Sun# set policy-options policy-statement ldp-routes term 1 from route-filter 172.30.5.1/32 exact
lab@Sun# set policy-options policy-statement ldp-routes term 1 then accept
lab@Sun# set protocols ldp egress-policy ldp-routes
《《《《... ... 把...ix subnet ...和...lo0...的地址放进...ldp database...里 .default inet.3里不会有自己的...lo0...的。... ...
...<<<egress policy...和...import policy...的区别就是前这可以给这些路由...label
<<<deactive lo0 from policy, then
lab@R1# run show ldp database
Input label database, 172.30.5.1:0--172.30.5.2:0
Label     Prefix
0     172.30.5.2/32
299792     172.30.5.3/32
299808     172.30.5.4/32
<<<... 172.30.5.1/32 disappeared.
lab@Sun# run show ldp database
Input label database, 172.30.5.1:0--172.30.5.2:0
Label     Prefix
299920     172.30.5.1/32
0     172.30.5.2/32
299872     172.30.5.3/32
299904     172.30.5.4/32
0     192.168.1.0/24... <<<<< ix routes was added into ldp with label
效果：
lab@Canopus# run show ldp database 《《...label...方向
Input label database, 172.30.5.3:0--172.30.5.4:0《《...r4...到...r3...路由带着什么...label...，是...r3...分给...r4...的
Label     Prefix
448928     172.30.5.1/32
448960     172.30.5.2/32
448944     172.30.5.3/32
0     172.30.5.4/32
448928     192.168.1.0/24
Output label database, 172.30.5.3:0--172.30.5.4:0《《...r3...到...r4...路由带着什么...label...，是...r4...分给...r3
Label     Prefix
408640     172.30.5.1/32
408608     172.30.5.2/32
0     172.30.5.3/32
408624     172.30.5.4/32
408608     192.168.1.0/24
lab@Canopus# run show route label ...xxx
lab@Canopus# run show route table mpls.0
《《《这两个是路由方向，和...label...反方向
所以...show route label...的时候都是这个路由带着什么...label...，所以这些...label...都要是下游...router...分配的，然而当去下游...router show ...这个...label...的时候是空，只能在...ldp database...里看到。在...local router...上只能...show...出来...output label...，因为是路由会带着这个...label...来。分配...label...并不会让这个...label...在...local router...上...show...出来，而是让他在下游...router...上有这个...label
R3input label...：
448928     192.168.1.0/24
R3 output label...：
408608     192.168.1.0/24
R3...：
lab@Canopus# run show route label 408608
408608             *[LDP/9] 00:27:33, metric 1
> to 172.30.0.13 via ge-0/0/4.123, Swap 0
《《《...nh...走...r2...，...swap 0...说明从...ix-2...出去...pop...掉...label...了
R4...：
lab@Arcturus# run show route label 448928
448928             *[LDP/9] 00:34:17, metric 1
> to 172.30.0.5 via ge-0/0/4.114, Swap 0
《《《走...r1...到...ix-1
所以实现了走两条...lsp...的目的。
lab@Canopus# run show route table inet.3
192.168.1.0/24     *[LDP/9] 00:33:56, metric 20
> to 172.30.0.13 via ge-0/0/4.123, Push 0
lab@Arcturus# run show route table inet.3
192.168.1.0/24     *[LDP/9] 00:38:29, metric 20
> to 172.30.0.5 via ge-0/0/4.114, Push 0
《《《可以看出走的是不同...lsp
5) ldp deaggregate
《《《加进来的...ix subnet...，他们和去其他...router...的...ldp routes...拥有共同...nh...，这样就会被分相同...label...。意味着走相同...ldp lsp...。
<<<ldp ...不支持...no-deaggregate-ttl...，但支持...deaggregate
--by default, junos  advertise same label for all prefixes with same nh or advertised by same nh roueter.
To change the behavior:
lab@R4# run show route 192.168.1.0
192.168.1.0/24     *[IS-IS/15] 00:24:45, metric 20
> to 172.30.0.5 via ge-0/0/4.114
lab@R4# run show route 172.30.5.2
172.30.5.2/32      *[IS-IS/15] 00:24:49, metric 15
> to 172.30.0.5 via ge-0/0/4.114
<<<<<...不同路由都是从...R4...从...R1...收到的，所以...nh...一样，即使他们属于不同...router advertise...的而...ldp...却只给他们分了一个...label
<<<<...比如...R4...他和...R2...没有...ldp session...所以...R2...来的所有路由都是通过...R1...学来的，...nh...也是...R1.
set protocols ldp deaggregate
<<<<on all routers
lab@R4# run show ldp database
Input label database, 172.30.5.4:0--172.30.5.3:0
Label     Prefix
299984     172.30.5.1/32
300016     172.30.5.2/32
0     172.30.5.3/32
299968     172.30.5.4/32
300000     192.168.1.0/24
>>>>This helps to load-balance mpls packets.
=========================================
