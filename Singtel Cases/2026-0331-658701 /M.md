
2026-0331-658701 [Singtel:Singtel-Megapop:MX-BD-04] SATs Customers having multiple connection issue on MX-BD-04 | Ticket #20251

https://speechify.com/auth/action?mode=resetPassword&oobCode=X-xLZRt9XkzZ_in1q3Bcp-B1vVjsp5T5G-EcSkYimpAAAAGdRrHPPw&apiKey=AIzaSyCv4J_Ocg6jrsSW-nUkSfPmuuxyUrcc-RM&lang=en

Hi Annabelle,
 
1./ We had a call yesterday with Singtel, NCS-Cisco vendor and their customer SATs.
 
Summary of the call:
Cisco has determined that the packet drops is due to CE (SATs) Cisco router COS buffer size issue
Kaycie suspects that there is an L2 issue between PE (MX-BD-04) to CE (SATs) Cisco router
Singtel to check further on this issue
 
2./ I have informed Singtel of Pranav’s analysis.
 
3./ Please keep this ticket under monitoring. Thank you.

-----------
Hi CFTS,
 
Thank you for joining the call.
 
We have concluded the call and determine that there are two issues causing the packet drops which is L2 issue in between PE and CE and the CE COS buffer issue.

---------

Hello Team,
For internal notes:

Queue priorities:
forwarding-classes {
queue 0 standard;
queue 1 business;
queue 2 premiumrt;
queue 3 nc_premiumnrt;
queue 6 ultra;
}

Queue 0 drops observed are expected as its the standard priority (it gets only left over resource):
10GE-standard-scheduler {
transmit-rate {
remainder;
}
buffer-size {
remainder;
}
}

Other queue buffer size:
10GE-business-scheduler {
transmit-rate percent 20;
buffer-size percent 20;
}

10GE-premiumrt-scheduler {
transmit-rate percent 10;
buffer-size percent 10;
}

10GE-nc_premiumnrt-scheduler {
transmit-rate percent 30;
buffer-size percent 30;
}

Drop profile:
drop-profiles {
standard {
interpolate {
fill-level [ 20 40 60 ];
drop-probability [ 0 50 100 ];
}
}
}

This confirms, drop starts at 20 percent of queue is filled and at 60, its aggressive.
Drops are intentionally configured behavior
Queue 0 uses “remainder” (no guarantee), and drop-profile allows early drops, hence drops are expected.
Confirmed on customer call as well, Cesar have joined the webex call and will continue.
# Case
Hi CFTS,

1./ Singtel reported their customer - SATS are unable to access Singtel's application services and there is service impact.

2./ The SATs impacted links are under ae35.
There appear to be no service impact for other ae35 unit links.

MX-BD-04: ae35.1318, ae35.1450
MX-TP-04: ae35.1280, ae35.1218, ae35.1351 and ae35.1477

ae35 on both routers have output errors drops


3./ I have uploaded the network diagram.
Link 1 (MX-BD-04) is the active link towards SATs customer while Link 2 (MX-TP-04) is the back up link.


4./ Singtel did the following troubleshooting.
- When the active link is still at Link 1 (MX-BD-04) and there is user traffic, Singtel performed a rapid ping test of 10,000 packets and there were random packet drops (without MTU1472 DF)
- Singtel have yet to test when there is no user traffic ping with MTU1472 DF

- Singtel swinged over the traffic to backup Link 2 (MX-TP-04) and did the below tests:
- When there is user traffic, rapid ping test of 10,000 packets will have random packet drops (without MTU1472 DF)
- When there is no user traffic, Singtel did a rapid ping test of 10,000 packets with MTU1472 DF and no packet drop


5./ I have uploaded the RSI and varlogs.
Please check if there are any anomaly ASAP.
