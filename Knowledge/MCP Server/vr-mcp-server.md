/volume/pfetools/mcp/vr-mcp
文件里定义了 3 个工具：
Tool	作用
vmx_manager	在 QPod 上启动/查询 VMX 虚拟路由器
params_generator	生成 .pl.params 拓扑配置文件
image_handler	获取/复制 Junos 镜像到 QPod

```
annaw@p-jtac-lnx01:/volume/pfetools/mcp/vr-mcp$ cat tools_config.json
{
    "tools": [
        {
            "name": "vmx_manager",
            "description": "Launch and manage virtual MX routers using .pl.params files across different platforms (EA/ZT/YT/XT).\n\nSupports two modes:\n- Show: Display VM IP addresses from qpods via SSH\n- Deploy: Launch virtual MX routers with automated post-deployment IP checking\n\nFeatures automated timeout handling (5 min), SSH key authentication, and post-launch VM IP inspection.\n\nSmart Image Handling: If no image is specified for deploy mode, automatically fetches latest VMDK image and copies to qpod.\n\nNote: Use params_generator tool first to create .pl.params files for new topologies.\n\nDefault QPod: Geo-based (America: q-pod08-vmm, Asia: elpod1-vmm)",
            "script": "tools_shim/vmx_manager.py",
            "parameters": {
                "show": {
                    "type": "boolean",
                    "default": false,
                    "description": "Show VM IP addresses from qpod"
                },
                "deploy": {
                    "type": "boolean",
                    "default": false,
                    "description": "Deploy/launch a virtual MX router"
                },
                "params": {
                    "type": "string",
                    "description": "Path to params file (.pl.params) [Required for deploy mode]"
                },
                "image": {
                    "type": "string",
                    "description": "Image location (optional for deploy mode). If not specified, automatically fetches latest VMDK image and copies to qpod"
                },
                "qpod": {
                    "type": "string",
                    "description": "QPod to use (default: geo-based - America: q-pod08-vmm, Asia: elpod1-vmm)"
                },
                "dry_run": {
                    "type": "boolean",
                    "default": false,
                    "description": "Generate config file but do not execute devtest (dry run mode)"
                }
            }
        },
        {
            "name": "params_generator",
            "description": "Generate .pl.params files for VMX router topologies. Supports all Juniper platforms with multi-FPC configurations and custom connections.",
            "script": "tools_shim/params_generator.py",
            "parameters": {
                "devices": {
                    "type": "string",
                    "description": "Comma-separated device list. Format: 'device_name:PLATFORM' (e.g., 'r1:VBALERION,r2:ALFAROMEO,spirent')\n\nSupported Platforms: ZT (FERRARI, MPC10), EA10G (EA, EAGLE, STOUT), HAMILTON (YT), ALFAROMEO (YT), LC304 (BUGATTI, MX304), LC301 (MODELX, MX301), MASERATI (XT), CYBERTRUCK, VBALERION, VAEGON\n\nUse 'spirent' for traffic generators. Multi-FPC: 'device:PLATFORM1:PLATFORM2'"
                },
                "connections": {
                    "type": "string", 
                    "description": "FPC-specific connection specification with mandatory slot addressing. Format: 'dev1[slot]-dev2[slot](count)'.\n\nExamples: 'dut[0]-r1[0](6)', 'dut[1]-spirent[0](2)', 'dut[0]-r1[1](4), r1[0]-spirent[0](2)'\n\nFPC slots start from 0. Multiple connections: comma/space separated.\nIf specified, overrides default chain topology. Unused FPC slots automatically get dummy interfaces for proper initialization.",
                    "default": ""
                },
                "output": {
                    "type": "string",
                    "description": "Custom output filename. If not specified, auto-generates timestamped filename in /var/tmp/ with format: {devices}_{timestamp}.pl.params",
                    "default": ""
                }
            }
        },
        {
            "name": "image_handler",
            "description": "Fetch and manage official Junos images from dev_common_branch or specific releases.\n\nSupports fetching latest VMDK images, last good images (marked by LG_S2C symlink), and copying to qpods.\n\nSupported image types:\n- VMDK: VMX platforms (VMX10008, VMX960, VMX304, VMX301, VMX9608)\n- PTX-Chassis: EVO PTX chassis platforms (VAEGON, VSCAPA, VOCTOMORE)\n- PTX-Fixed: EVO PTX fixed platforms (VBALERION, VWHISTLER)\n\nDefault QPod: Geo-based (America: q-pod08-vmm, Asia: elpod1-vmm)\n",
            "script": "tools_shim/image_handler.py",
            "parameters": {
                "type": {
                    "type": "string",
                    "description": "Image type. Supported types: 'vmdk' (VMX platforms), 'ptx-chassis' (EVO PTX chassis platforms), 'ptx-fixed' (EVO PTX fixed platforms)",
                    "default": "vmdk"
                },
                "release": {
                    "type": "string",
                    "description": "Release version (e.g., 25.4, 26.1). If not specified, uses latest from dev_common_branch"
                },
                "latest": {
                    "type": "integer",
                    "description": "Number of latest images to show (default: 1). Cannot be used with last_good",
                    "default": 1
                },
                "last_good": {
                    "type": "boolean",
                    "description": "Show last good image (marked by LG_S2C symlink). Cannot be used with latest",
                    "default": false
                },
                "copy_to_qpod": {
                    "type": "string",
                    "description": "Copy image to specified qpod. If set to 'true' or '1', uses default geo-based qpod (America: q-pod08-vmm, Asia: elpod1-vmm). Can only copy single images (use latest=1 or last_good=true)"
                }
            }
        }
    ]
}

```



```
python3 -V
python3 -m venv ~/venvs/vr-mcp
source ~/venvs/vr-mcp/bin/activate
python -m pip install --no-index --find-links ~/vr-mcp-wheels-linux "mcp[cli]"
nohup ~/venvs/vr-mcp/bin/python ~/vr_mcp_server.py --port 30031 --host 0.0.0.0 > ~/vr-mcp.log 2>&1 &
echo $!
tail -n 60 ~/vr-mcp.log
hostname -I | awk '{print $1}'

```
