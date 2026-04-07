# Trio MPC

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

Trio MPC
// note the MACRO is "VMX_MPC_I2CID" for Trio chip set
VMX_MPC_START(q1_mpc, 0)
VMX_MPC_INSTANCE(q1_mpc, VMX_MPC_DISK, VMX_MPC_I2CID, 0)
VMX_CONNECT(GE(0, 0, 0), private6)
VMX_CONNECT(GE(0, 0, 1), private37)
VMX_CONNECT(GE(0, 0, 9), private10)
VMX_CONNECT(XE(0, 2, 0), private1)
VMX_CONNECT(XE(0, 3, 0), private2)
VMX_MPC_END
VMX_CHASSIS_END
FPC 0                     BUILTIN      BUILTIN           Virtual FPC
CPU            Rev. 1.0 RIOT-LITE    BUILTIN
MIC 0                                                               Virtual 20x 1GE(LAN) SFP
PIC 0                 BUILTIN      BUILTIN           Virtual 10x 1GE(LAN) SFP
PIC 1                 BUILTIN      BUILTIN           Virtual 10x 1GE(LAN) SFP
MIC 1                                                               Virtual 4x 10GE(LAN) XFP
PIC 2                 BUILTIN      BUILTIN           Virtual 2x 10GE(LAN) XFP
PIC 3                 BUILTIN      BUILTIN           Virtual 2x 10GE(LAN) XFP
