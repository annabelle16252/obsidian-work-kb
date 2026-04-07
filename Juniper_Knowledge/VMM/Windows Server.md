# Windows Server

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

Windows Server
vm "WindR_VM1" {
hostname "WindR_VM1";
WindR_VM1
memory 8192;
ncpus 4;
interface "em0" { bridge "external"; };
interface "em1" { bridge "private7"; };
interface "em2" { bridge "private10"; };
};
