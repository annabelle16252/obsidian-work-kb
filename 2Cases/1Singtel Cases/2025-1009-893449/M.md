Noted Annabelle,

Okay PAYALEBAR-PostPaid-Private-5G &JURONGEAST-PostPaid-Private-5G was for the ae20, which was had been migrated to SPC3-CGN

===========================

Dear Francis,

Yes, I saw the refixes under NAT. But what confused me is you told me now traffic go via ae10.

<<<  
ae0 and 1 to subscriber, but these interfaces should have no traffic already, ae10 subscriber traffic (subscriber traffic)

<<<

But from configuration , the input filter INSIDE-4G which doesnt include allow PAYALEBAR-PostPaid-Private-5G &JURONGEAST-PostPaid-Private-5G. Instead it's JURONGEAST-PostPaid-Private-4G & PAYALEBAR-PostPaid-Private-4G.

Anyway, as long as you can confirm QUIC traffic prefixes will not be blocked that's fine.

I am on holiday next Monday, we can continue to discuss if there's any further concern on the two cases.

==================

Dear Francis,

So you mean 'source-prefix-list'in use for QUIC are as below? It's kind of hard for me to guess which prefixes will go through which inside interfaces.

Anyway, my guess is for ae0.274 & ae1.273, you need filter input BEDOK-INSIDE to allow below prefixes:

<<<

            term BPJang-BACKUP {

                from {

                    source-prefix-list {

                        BPJang-Public;

                        BPJang-PostPaid-Private;

                        BPJang-PrePaid-Private;

                    }

                }

                then {

                    forwarding-class best-effort;

                    routing-instance Site-Backup; <<<< but to other routing-instance??

                }

            /* Private IP to be NATed, continue to INSIDE */

            term NAT {

                from {

                    source-prefix-list {

                        BEDOK-PostPaid-Private;

                        BEDOK-PrePaid-Private;

                    }

                }

                then {

                    forwarding-class best-effort;

                    accept;

                }

<<<

For ae10.113, you need  input filter INSIDE-4G,filter to allow below prefixes:

<<<

        filter INSIDE-4G {

            term BPJang-BACKUP-NAT {

                from {

                    source-prefix-list {

                        JURONGEAST-PostPaid-Private-4G;

                        JURONGEAST-PrePaid-Private-4G;

                    }

                }

                then {

                    forwarding-class best-effort;

                    accept;

                }

            }

            term NAT {

                from {

                    source-prefix-list {

                        PAYALEBAR-PostPaid-Private-4G;

                        PAYALEBAR-PrePaid-Private-4G;

                    }

                }

                then {

                    forwarding-class best-effort;

                    accept;

                }

            }

<<<

As long as those are all the prefixes for QUIC traffic, that would be fine.

Since there's no special COS configuration, and all the QUIC traffic will be put into BE queue, as long as no sudden spike abnormal traffic, that's should be fine.That's depends on the production enviorment.

==================

Hi Annabelle,

For Number 3: if  the prefixes are not configured, traffic will not be accepted, routes will not be accepted, and they will not be NATed.

The same prefixes name has been used for firewall, bgp, and NAT.

=================================

Dear Francis,

I think we just make sure that our configuration CGNAT processes the QUICK 443/UDP,  doesn't have a blocking protocol first for  443/UDP, and no rate limiting or policing.

<Annabelle> I didnt find any issue with current available information.

1.For NAT rules MS300 MS310 MS320 MS330, yes it should fall into this term and QUIC will not be bolcked.

<<<

TCP and UDP should match one of these rules per MS-MPC MIC.

           term PostPaid-TCP-UDP {

                from {

                    source-prefix-list {

                        BEDOK-PostPaid-Private;

                        BPJang-PostPaid-Private;

                        PAYALEBAR-PostPaid-Private-5G;

                        JURONGEAST-PostPaid-Private-5G;

                    }

                    application-sets TCP-UDP;

                }

                then {

                    translated {

                        source-pool BEDOK-2-MS300-PostPaid-POOL;

                        translation-type {

                            napt-44;

                        }

                        address-pooling paired;

                    }

                }

            }

    application-set TCP-UDP {

        application TCP;

        application UDP;

    }

          application UDP {

        protocol udp;

        destination-port 1-65535;

        inactivity-timeout 120;

    }

          application TCP {

        protocol tcp;

        destination-port 1-65535;

        inactivity-timeout 1800;

    }

<<<

2.For ISP facing interfaces ae4.0 ae5.0, there's filter input OUTSIDE which allowed the NATed address:

<<<<

            /* return traffic for NAT IP to be NATed back, continue to OUTSIDE for reverse NAT */

            term NAT {

                from {

                    destination-prefix-list {

                        BEDOK-CGN2-NAT-Pools;

                        BEDOK-CGN1-NAT-Pools;

                    }

                }

                then {

                    forwarding-class best-effort;

                    accept;

                }

<<<<

3.For subscriber input interfaces, there are filter as below:

<<<<

        interface ae0.274;

        interface ae1.273;

                filter {

                    input BEDOK-INSIDE;

        interface ae10.113;

                filter {

                    input INSIDE-4G;

<<<<

Since I dont know what's your source prefixes, please make sure no QUIC traffic will be filter/discard here.

4.Policer is not applied to relative interfaces. No special COS configuration.

==============================

Hi Anabelle,

I think we just make sure that our configuration CGNAT processes the QUICK 443/UDP,  doesn't have a blocking protocol first for  443/UDP, and no rate limiting or policing.

The AE interfaces are similar to the SPC3 CGNAT except for traffic from subscriber, ae4 -Primary ISP, ae5- backup ISP, ae2 CGN interlink, ae0 and 1 to subscriber, but these interfaces should have no traffic already, ae10 subscriber traffic (subscriber traffic)

I had checked that the IP 111.65.48.0/24 is assigned for MS-MPC slot 3  NAT Pools of BPJANG_CGN_2

TCP and UDP should match one of these rules per MS-MPC MIC.

           term PostPaid-TCP-UDP {

                from {

                    source-prefix-list {

                        BEDOK-PostPaid-Private;

                        BPJang-PostPaid-Private;

                        PAYALEBAR-PostPaid-Private-5G;

                        JURONGEAST-PostPaid-Private-5G;

                    }

                    application-sets TCP-UDP;

                }

                then {

                    translated {

                        source-pool BEDOK-2-MS300-PostPaid-POOL;

                        translation-type {

                            napt-44;

                        }

                        address-pooling paired;

                    }

                }

            }

    application-set TCP-UDP {

        application TCP;

        application UDP;

    }

          application UDP {

        protocol udp;

        destination-port 1-65535;

        inactivity-timeout 120;

    }

          application TCP {

        protocol tcp;

        destination-port 1-65535;

        inactivity-timeout 1800;

    }

========================================

Dear Francis,

Could you help to provide below information?

1.Topology

2.You mean subscriber is from 111.65.48.0/24 and QUIC traffic is passing  BEDOK_CGN_2?

3.Please share which NAT rule does QUIC traffic use?

Kindly provide as much as information for CFTS review.

============================

Hi Annabele,

I had uploaded the health check for BEDOK and BPJANG for MS-MPC CGNAT.

the NATED IP  of the test subscriber was in  111.65.48.0/24 belongs to BEDOK_CGN_2

We do something similar to the previous case to check that QUIC has not been blocked or filtered on CGNAT

============================

Dear Francis,

This is Annabelle Wang from Japan CFTS Team and I have taken the ownership of this case.

I will go through the case and provide you and update later.