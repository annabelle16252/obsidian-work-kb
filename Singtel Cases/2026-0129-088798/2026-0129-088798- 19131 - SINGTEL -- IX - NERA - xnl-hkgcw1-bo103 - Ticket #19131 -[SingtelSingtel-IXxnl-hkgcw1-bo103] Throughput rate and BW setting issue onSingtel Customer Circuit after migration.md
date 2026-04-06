---
aliases:
  - |-
    2026-0129-088798
    - 19131 - SINGTEL -- IX - NERA - xnl-hkgcw1-bo103 - Ticket #19131 -
    [Singtel:Singtel-IX:xnl-hkgcw1-bo103] Throughput rate and BW setting issue on
    Singtel Customer Circuit after migration
---
Dear Neo,

Yes, as we have confirmed with internal Team & DE team, it's not officlally supported.

Not sure on which version and which MPC you see it's working.  What we know that there was some incorrect code fix for NG MPCs which makes commit pass. But Juniper cannot assure it works fine.

==============================

Hello Annabelle,

1./ Apologies, I just remembered that the 1G and 10G mixed speed AE was working in the past.

I just want to confirm if Juniper does not officially support such combination?

Thank you.

==============================

Dear Neo,

'link-speed mixed' mode for ae interfaces only support 10GE/40GE/100GE and 1GE and our offical document for AE mixed speed never mentions 1GE:

[https://www.juniper.net/documentation/us/en/software/junos/interfaces-ethernet/topics/topic-map/aggregated-ethernet-interfaces-lacp-configure.html#jd0e2507](https://www.juniper.net/documentation/us/en/software/junos/interfaces-ethernet/topics/topic-map/aggregated-ethernet-interfaces-lacp-configure.html#jd0e2507)

I also checked some internal documents and confirmed the above.  We would not recommend you to use mixed speed with 1GE interface as part of ae-.

fyi, for ALOHA(NG) cards only you may be able to commit 1GE for ae mixed speed, this is due to some incorrect code fix and it's not expected.

I will put this case under monitoring.

=========================

Dear Neo,

This is Annabelle Wang from Japan CFTS Team and I will be assisting you with your case.

I will go through the case and provide you and update later.