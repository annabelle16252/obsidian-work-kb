# AMS

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

AMS
AMS logical interfaces doesnt show correct bandwidth, no functional impact
KB81673
https://gnats.juniper.net/web/default/1550571#audit_tab1. As mentioned earlier, when bandwidth is configured under ifl, bandwidth value is reflecting the configured value.2. When not configured, this field is not handled. It might be having a stale value. Hence the random values. if the stale value matches ifd speed, bandwidth is not shown.3. Bandwidth field is not used by AMS in any way. So it doesn't have functional impact. So if bandwidth display is concern, it can be configured under ifl.
======================
Configuring Load Balancing on AMS Infrastructure
https://www.juniper.net/documentation/us/en/software/junos/interfaces-next-gen-services/interfaces-adaptive-services/topics/concept/services-configuring-load-balancing.html
Network Address Translation (NAT) has been programmed as a plug-in and is a function of load balancing and high availability. The plug-in runs on AMS infrastructure. All flows for translation are automatically distributed to different services PICs that are part of the AMS infrastructure.
Starting in Junos OS Release 19.3R2, AMS interfaces are supported with the MX-SPC3.
Currently, service provisioning is done using service-set model and one service-set can be bound to single service interface/pic [or single RSP interface]
The resources need to be manually split (NAT pools, rules etc.,) by end user and provisioned across multiple PICs through different service-sets to achieve load balancing
With AMS, service can be load-balanced across all members of the AMS bundle
Resources associated with the service are dynamically split and provisioned to the member interfaces
Only supported with mp-sdk based service cards
“ams” and member of ams interface called “mams”
“mams” has to be part of ams bundle to service traffic, configuration on mams is not supported
Configure the hash keys at service-set level (for interface style service) and AMS unit level (for NH style service)
Unit 0 cannot be configured on AMS interface as it is the default unit of ms- interface
Addressing is not supported on AMS interface
