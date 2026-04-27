# PTX10003 - Brackla

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

PTX10003 - Brackla
Brackla: PTX&QFX1003
# Command #
Brackla AFT login:
start shell
vty fpc0
Brackla PFE login:
start shell
vty fpc0.0
Brackla PFE login:
start shell user root
cd /usr/bin/
cda-jspec
Routing Engine 0 REV 05   750-080695   BCAG7020          RE-JNP10003-80C
FPC 0            REV 07   750-077005   BCAG8280          FPC-JNP10003-MEZZ-C
SIB 0                     BUILTIN      BUILTIN           SIB-JNP10003
CB 0             REV 06   750-077001   BCAG8112          Control Board
FPC 3 
ZX 0 - ZX 3 (PE 0 - PE 7) 
16T 
FPC 2 
ZX 4 - ZX 7 (PE 8 - PE 15) 
FPC 1 
ZX 8 - ZX 11 (PE 16 - PE 23) 
FPC O 
ZX 12 - ZX 15 (PE 24 - PE 31) 
8T 
FPC 1 
ZX 8 - ZX 11 (PE 16 - PE 23) 
FPC O 
ZX 12 - ZX 15 (PE 24 - PE 31) ...FPC 3 
ZX 0 - ZX 3 (PE 0 - PE 7) 
16T 
FPC 2 
ZX 4 - ZX 7 (PE 8 - PE 15) 
FPC 1 
ZX 8 - ZX 11 (PE 16 - PE 23) 
FPC O 
ZX 12 - ZX 15 (PE 24 - PE 31) 
8T 
FPC 1 
ZX 8 - ZX 11 (PE 16 - PE 23) 
FPC O 
ZX 12 - ZX 15 (PE 24 - PE 31)
> show platform dmf board /Chassis[0]/Fpc[0] detail
> show platform dmf board /Chassis[0]/Fpc[0] detail
> show chassis jspec asics
# CDA #
Initializes ZX ASIC and HBM
Manages ZX PFE blocks
CDA-EVO layer introduced to interface CDA-Common with EVO
Participates in distributed DMF sequence to initialize FPCs
ZX drivers remains agnostic to EVOisms
Capable of initializing ASICs in parallel – reduces service restore time
AFT CLI (cli-pfe) is the debug interface
# PICD #
Port management – Publishes port state DDS objects
Transceiver Optics and LED management
WAN side link training
MAC / PCS / Benes / WANIO programming
