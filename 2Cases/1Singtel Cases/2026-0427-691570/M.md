2026-0427-691570 - 20577 - SINGTEL -- IX - NERA -[Singtel:Singtel-IX:xn-sineq4-bo301] Checking for OpenSSH 10.3 CVE-2026-35385 for 23.2R2-S2.5| Ticket #20577


https://engage.cloud.microsoft/main/org/hpe.com/groups/eyJfdHlwZSI6Ikdyb3VwIiwiaWQiOiIyNTAyNjM5MzcwMjQifQ/all



Dear Tong,

CVE-2026-35385 , CVE-2026-35386, CVE-2026-35387, CVE-2026-35388, CVE-2026-35414 have been added into JSA80837.


-------------


Hi Ulf/Naveen/Travis,
JSA80837 has been updated with the necessary details on these 5 SSH CVEs.

-Melvina

--------------------

sirt@juniper.net

Hi SIRT team, 

Could you let us know if CVE-2026-35385 , CVE-2026-35386, CVE-2026-35387, CVE-2026-35388, CVE-2026-35414 will be included in JSA80837.


--------------


Hi SIRT team, 

Could you let us know if CVE-2026-35385 , CVE-2026-35386, CVE-2026-35387, CVE-2026-35388, CVE-2026-35414 will be included in JSA80837.

Regards,

Naveen Kumar Sivakumaran

ARC Engineer 



-------------

Hi Naveen,
 
Please check with the SIRT team if the five CVEs will be included in the following JSA.
 
JSA80837:
https://supportportal.juniper.net/s/article/2024-05-Reference-Advisory-Junos-OS-and-Junos-OS-Evolved-Multiple-CVEs-reported-in-OpenSSH
 
--------------



Hi,

Junos versions- 23.4R2-S5, 21.2R3-S7.7, 21.2R3-S8.5, 21.4R3-S5.17, 23.2R2-S2.5 are not impacted by the CVE-2026-35385 as this CVE is not exploitable on Junos/EVO devices. Please refer to PR#1951875 - AT#51 for more information on this.

Thanks,
Melvina

------------------

Dear Tong,

We have checked internally on this and found Junos version 23.2R2-S2.5 is not affected by the vulnerability CVE-2026-35385.

------------

Dear Tong,

This is Annabelle Wang from Japan CFTS Team and I will be assisting you with your case.
I will go through the case and provide you and update later.



# Case
Hi CFTS,

Singtel is asking to check if version 23.2R2-S2.5 is affected by CVE-2026-35385.
Please check on this. Thank you.

- In OpenSSH before 10.3, a file downloaded by scp may be installed setuid or setgid, an outcome contrary to some users' expectations, if the download is performed as root with -O (legacy scp protocol) and without -p (preserve mode). (CVE-2026-35385)