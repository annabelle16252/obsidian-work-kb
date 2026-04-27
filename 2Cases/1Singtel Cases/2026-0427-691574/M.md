
 2026-0427-691574 - 20577 - SINGTEL -- IX - NERA -[Singtel:Singtel-IX:xn-sineq4-bo301] Checking for OpenSSH 10.3 CVE-2026-35387 for 23.2R2-S2.5| Ticket #20577

# Case
Hi CFTS,

Singtel is asking to check if version 23.2R2-S2.5 is affected by CVE-2026-3538.
Please check on this. Thank you.

- OpenSSH before 10.3 can use unintended ECDSA algorithms. Listing of any ECDSA algorithm in PubkeyAcceptedAlgorithms or HostbasedAcceptedAlgorithms is misinterpreted to mean all ECDSA algorithms. (CVE-2026-35387)