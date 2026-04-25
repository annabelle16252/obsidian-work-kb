# WebUI

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

WebUI
docker重装...(或者换电脑)...，上传到webui里的knowledge和在webui上的model都会跟着丢。需要作以下定期备份：
1. 定期备份（建议每周或改完 model 后跑一次）
cd /Users/annaw/Annabelle/AI/Cursor/202501/ollama-rag
./backup_webui.sh
会在... webui_backups/ 下生成类似：open_webui_data_20250218_143000.tar.gz
建议把最新的备份再拷到... U 盘或云盘
2. 需要恢复时（例如重装 Docker、换电脑后）
cd /Users/annaw/Annabelle/AI/Cursor/202501/ollama-rag
./restore_webui.sh
不传参数会列出已有备份...，按提示选一个
或直...接：./restore_webui.sh open_webui_data_20250218_143000.tar.gz
脚本会停掉 open...-webui → 恢复 volume → 再启动，完成后刷新 http://localhost:3000 即可
