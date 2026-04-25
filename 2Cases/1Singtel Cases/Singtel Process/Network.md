# Share Folder #
[https://junipernetworks.sharepoint.com/teams/CSS/cfts/apac/Shared%20Documents/Forms/Standard.aspx?id=%2Fteams%2FCSS%2Fcfts%2Fapac%2FShared%20Documents%2FSingTel&viewid=36b4edee%2Da804%2D4c98%2Da7ba%2Dac49bf629c2d&FolderCTID=0x01200048E5B06096045A4694359A898417F197&web=1](https://junipernetworks.sharepoint.com/teams/CSS/cfts/apac/Shared%20Documents/Forms/Standard.aspx?id=%2Fteams%2FCSS%2Fcfts%2Fapac%2FShared%20Documents%2FSingTel&viewid=36b4edee%2Da804%2D4c98%2Da7ba%2Dac49bf629c2d&FolderCTID=0x01200048E5B06096045A4694359A898417F197&web=1)


# NAT #
|   |
|---|
|NAT|
|translation-type source dynamic|
|stateful-firewall-rules|
|nat-rules|
|next-hop-service|
|twice-nat-44|
|nat pool ( 1 IP , multiple port)|

Twice NAT44：同时修改 源地址 + 目的地址，甚至连端口也可以改。

STM
IPv4 Dyn. SA Port Translation
IPv4-IPv6 Dyn. SA Port Translation + DA Prefix 


MS-MIC-16G

MS-MPC-128G

SPC3 (Testing)

CASE

2025-0207-454819 Nated Return Traffic Dropped when missing SPC3 in the ams interface

2023-0808-746039

Traceroute Route next from CGN no NAT session

2024-1008-279690

[:Singtel-Megapop:MX-AR-03] Megapop Ticket Number: INC000024882436 | TKT000000368157 NAT Pool not showing in the routing table | Ticket #9112

2025-0226-614507

Subriber CGNAT Traffic Dropped

2023-1218-031980

ams interface speed bandwidth adjustment for SNMP

CGNAT case 2025-0904-843647



# RSVP-TE
Hostname: LONHE-ER6

Model: mx480

Junos: 23.2R2-S3.8

CASE: 2025-0922-867063

**install address <prefix> 

[https://www.juniper.net/documentation/us/en/software/junos/cli-reference/topics/ref/statement/install-edit-protocols-mpls.html](https://www.juniper.net/documentation/us/en/software/junos/cli-reference/topics/ref/statement/install-edit-protocols-mpls.html)

Associate one or more prefixes with an LSP. When the LSP is up, all the prefixes are installed as entries into the inet.3.

# set protocols mpls label-switched-path a-centauri-to-sirius install 192.168.1.0/24  <<<

Install knob: add to inet3

install active knob: add this lsp route into inet0 as well

inet.3: 359 destinations, 366 routes (359 active, 0 holddown, 0 hidden)

+ = Active Route, - = Last Active, * = Both

101.234.63.57/32   *[RSVP/7/1] 01:53:46, metric 262

                    >  to 101.234.61.13 via xe-1/0/0.0, label-switched-path BOI-LONHE-ER6-EIG-MUMTT-ER1

                       to 101.234.61.221 via xe-0/1/0.0, label-switched-path BOI-LONHE-ER6-EIG-MUMTT-ER1

                    [RSVP/8/1] 01:54:45, metric 262

                    >  to 101.234.61.1 via xe-0/0/0.0, label-switched-path BOI-LONHE-ER6-EIG-MUMTT-ER1

                       to 101.234.61.221 via xe-0/1/0.0, label-switched-path BOI-LONHE-ER6-EIG-MUMTT-ER1

                    [LDP/9] 1w2d 17:15:56, metric 262

                       to 101.234.61.1 via xe-0/0/0.0, Push 22

                    >  to 101.234.61.13 via xe-1/0/0.0, Push 22

    mpls {

        statistics {

            file LSP-stats size 10m files 10;

            interval 300;

        }

        log-updown {

            syslog;

            trap;

            trap-path-down;

            trap-path-up;

        }

        no-propagate-ttl;<<<<

        label-switched-path MVPN-P2MP-1M {

            template;

            bandwidth 1m;

            no-cspf;

            p2mp; <<<<

        }

        label-switched-path BOI-LONHE-ER6-EIG-MUMTT-ER1 {

            no-install-to-address;

            to 101.234.58.23;

            install 101.234.63.57/32;   <<<<

            fast-reroute;

            primary PRI-PATH-LONHE-ER6-EIG-MUMTT-ER1 {

                preference 7;

            }

            secondary SEC-PATH-LONHE-ER6-SMW4-MUMTT-ER1 {

                preference 8;

                standby;

            }

            secondary SEC-PATH-LONHE-ER6-IGP-MUMTT-ER1 {

                standby;

            }

        }

        ipv6-tunneling;<<<<

        interface xe-0/0/0.0;

        interface xe-1/0/0.0;

        interface xe-0/1/0.0;

    }

    ospf {

        traffic-engineering;

    pim {

        rp {

            static {

                address 202.163.59.100;

            }

        }

        interface xe-0/0/0.0 {

            mode sparse;<<<<

        }

    rsvp {

        interface xe-0/0/0.0 {

        }

        interface xe-1/0/0.0 {

        }

        interface xe-0/1/0.0 {

        }

# Segment routing
Static SR-TE

> show spring-traffic-engineering lsp  <<< 看SR—TE lsp

To              State     LSPname

10.233.254.64-1<c> Up     LSP-SRTE:01.tdf01.lab--hgw21.lab

10.233.254.133-1<c> Up    LSP-SRTE:01.tdf01.lab--imse03_04.lab

# run show route 101.234.3.132   <<< inet.3里有给接下来各个跳分的label       

inet.0: 23 destinations, 24 routes (23 active, 0 holddown, 0 hidden)

+ = Active Route, - = Last Active, * = Both

101.234.3.132/32   *[IS-IS/18] 2d 20:06:03, metric 400

                    >  to 101.234.16.18 via xe-1/0/5:0.0

inet.3: 4 destinations, 5 routes (4 active, 0 holddown, 0 hidden)

+ = Active Route, - = Last Active, * = Both

101.234.3.132/32   *[SPRING-TE/8] 2d 00:07:22, metric 1, metric2 400

                    >  to 101.234.16.18 via xe-1/0/5:0.0, Push 32, Push 201101(top)<<<ingress push all labels

                    [L-ISIS/14] 2d 20:01:57, metric 400

                    >  to 101.234.16.18 via xe-1/0/5:0.0, Push 401012

                       to 101.234.16.20 via xe-1/0/5:1.0, Push 401012

nh to 101.234.16.18上check label 201101: <<< 根据上面label可以按hop来看

annaw@lab-mx480-3d-02-re0# run show route table mpls.0 label 201101

201101             *[L-ISIS/14] 2d 20:25:55, metric 0

                    >  to 101.234.16.7 via xe-1/1/1.0, Pop     

                       to 101.234.16.9 via xe-1/1/17.0, Swap 401011

201101(S=0)        *[L-ISIS/14] 2d 20:25:55, metric 0

                    >  to 101.234.16.7 via xe-1/1/1.0, Pop     

                       to 101.234.16.9 via xe-1/1/17.0, Swap 401011

nh to 101.234.16.7上check label 32:

annaw@lab-mx204-01> show route table mpls.0 label 32

32                 *[L-ISIS/14] 2d 20:31:44, metric 0

                    >  to 101.234.16.1 via et-0/0/0.0, Pop     

                       to 101.234.16.6 via xe-0/1/3.0, Swap 401012

32(S=0)            *[L-ISIS/14] 2d 20:31:44, metric 0

                    >  to 101.234.16.1 via et-0/0/0.0, Pop     

                       to 101.234.16.6 via xe-0/1/3.0, Swap 401012

<<<<

protocols {

    isis {

        interface xe-1/0/5:0.0 {

            level 2 {

                ipv4-adjacency-segment {

                    protected label 201601;

                    unprotected label 200601;

                }

            }

        }

        interface xe-1/0/5:1.0 {

            level 2 {

                ipv4-adjacency-segment {

                    protected label 201602;

                    unprotected label 200602;

                }

            }

        }

        traffic-engineering l3-unicast-topology;

    }

    mpls {

        label-range {

            static-label-range 200000 299999;

        }

    }

    source-packet-routing {

        segment-list SL-via-mx480-S-S {

            auto-translate;

            HOP1 ip-address 101.234.16.18;

            HOP2 ip-address 101.234.16.7;

            HOP3 ip-address 101.234.16.1;

        }

      source-routing-path PATH-SL-via-mx480-S-S {

            to 101.234.3.132;

            primary {

                SL-via-mx480-S-S;

            }

        }

<<<<

# run show spring-traffic-engineering lsp

To                        State        LSPname

101.234.3.132             Up           PATH-SL-via-mx480-S-S

# run show spring-traffic-engineering lsp detail

E = Entropy-label Capability

Name: PATH-SL-via-mx480-S-S

  Tunnel-source: Static configuration

  Tunnel Forward Type: SRMPLS

  To: 101.234.3.132

  Te-group-id: 0

  State: Up

    Path: SL-via-mx480-S-S

    Path Status: NA

    Outgoing interface: xe-1/0/5:0.0

    Auto-translate status: Enabled Auto-translate result: Success

    Compute Status:Disabled , Compute Result:N/A , Compute-Profile Name:N/A

    BFD status: N/A BFD name: N/A

    BFD remote-discriminator: N/A

    Segment ID : 128

    ERO Valid: true

      SR-ERO hop count: 3

        Hop 1 (Strict):

          NAI: IPv4 Adjacency ID, 0.0.0.0 -> 101.234.16.18

          SID type: 20-bit label, Value: 201601

        Hop 2 (Strict):

          NAI: IPv4 Adjacency ID, 0.0.0.0 -> 101.234.16.7

          SID type: 20-bit label, Value: 201101

        Hop 3 (Strict):

          NAI: IPv4 Adjacency ID, 0.0.0.0 -> 101.234.16.1

          SID type: 20-bit label, Value: 32

Total displayed LSPs: 1 (Up: 1, Down: 0, Initializing: 0)
