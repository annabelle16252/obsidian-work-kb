# vSpirent

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

vSpirent
vm "vspirent" {
hostname "vspirent";
basedisk "/vmm/data/base_disks/spirent/spirent-5.22.0676.img";
interface "em0" { bridge "external"; };
interface "em1" { bridge "private37"; };
};
