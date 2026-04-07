# EA MPC

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

EA MPC
set chassis fpc 1 pic 0 pic-mode 100G
set chassis fpc 1 pic 0 channelization
set chassis network-services enhanced-ip <<< need to have this config
MRATE-6xQSFPP-XGE-XLGE-CGE <<< XGE-XLGE-CGE...：...10G-40G-100G
设成100G的时候...，只有...et-x/0/2 et-x/0/5 可以用
VMX_MPC_START(r1_MPC1, 1)
VMX_MPC_INSTANCE(r1_MPC1, VMX_DISK, VMX_EA_MPC_I2CID, 1)
VMX_CONNECT(ET(1, 0, 0, 0), private2)
VMX_CONNECT(ET(1, 0, 1, 0), private2)
VMX_CONNECT(ET(1, 0, 2, 0), private3)
VMX_CONNECT(ET(1, 0, 3, 0), private3)
VMX_MPC_END
VMX_MPC_START(r1_MPC1, 1)
VMX_MPC_INSTANCE(r1_MPC1, VMX_DISK, VMX_EA_MPC_I2CID, 1)
VMX_CONNECT(ET(1, 0, 0, 0), private2)   // 100g connection //
VMX_CONNECT(ET(1, 0, 1, 0), private2)   // 100g connection //
VMX_CONNECT(ET(1, 0, 2, 0), private3)   // 100g connection //
VMX_CONNECT(ET(1, 0, 3, 0), private3)   // 100g connection //
VMX_MPC_END
// define MPC1 as EA based MPC
// note the MARCO is "VMX_EA_MPC_I2CID" for EA chip set
// port number is set in the third column for 40G or 100G port
// the fourth column should always be 0 for ET links
VMX_MPC_START(r1_MPC2, 2)
VMX_MPC_INSTANCE(r1_MPC2, VMX_DISK, VMX_EA_MPC_I2CID, 2)
VMX_CONNECT(XE_CHAN(2, 0, 0, 0), private4)   // 10g CONNECTION //
VMX_CONNECT(XE_CHAN(2, 0, 0, 1), private4)   // 10g CONNECTION //
VMX_CONNECT(XE_CHAN(2, 0, 0, 2), private5)   // 10g CONNECTION //
VMX_CONNECT(XE_CHAN(2, 0, 0, 3), private5)   // 10g CONNECTION //
VMX_MPC_END
// 10g link could be used on EA based as well
// note the link name changed, and the fourth column is used
For XE channelized interfaces : VMX_CONNECT(XE_CHAN(0, 0, 0, 0), private1) , in this 4th parameter will take 0, 1, 2, 3 to represent chennels.​
VMX_EA_MPC_I2CID
For XE channelized interfaces : VMX_CONNECT(XE_CHAN(0, 0, 0, 0), private1) , in this 4th parameter will take 0, 1, 2, 3 to represent chennels.​
For ET interfaces (40G or 100G) : VMX_CONNECT(ET(0, 0, 0, 0), in case ET interface 4th parameter should be zero always.
