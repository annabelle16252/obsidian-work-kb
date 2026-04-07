# Junos branching model

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

Junos branching model
single-sourcing
commit in junos will also be used in EVO
Source mode: commit in junos will automatically gets translated into EVO.
Binary mode:Binaries are built in JUNOS using EVO tool chain to be consumed by EVO (ie. rpd).
Pointer update consists of pointing to new publish location of a component.
UCC is gating process in JUNOS and EVO commit pipelines to ensure the single source commit happening in one code base does not break the other code base.
23.4R2-S4-EVO-TO-JUNOS-HELPER <<< EVO的问题在junos/SVN side fix
JUNOS DCB <<---->> EVO Master
21.2R3-S1-EVO-TO-JUNOS-HELPER  PR :1841098
21.2R3-S1-JUNOS-TO-EVO-HELPER
Junos-EVO_TOI_SM_d02
Junos-EVO_TOI_SM_d02
