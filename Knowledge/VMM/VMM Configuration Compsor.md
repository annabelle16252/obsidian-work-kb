
# To-do
1.vixia template different with pbuilder. GLO-1242257-pod21-test.vmm
2.Win_R VM
3.redbull
4.ptx5k
5.vbrackla only one mpc?
6.vscapa how to add another fpc? param file?


# github
https://github.hpe.com/ying-wang/VMM-Config-Composer

User Guide:
file:///Users/annaw/Annabelle/AI/Cursor/202501/USER_GUIDE.html

Tool：
file:///Users/annaw/Annabelle/AI/Cursor/202501/index.html




For HPE Users Only:
In Terminal: 
 1. Generate the key (specify file to avoid overwrite prompt):
 ssh-keygen -t ed25519 -C "user-name@hpe.com" -f ~/.ssh/id_ed25519
 2. Set permissions:
 chmod 600 ~/.ssh/id_ed25519 && chmod 644 ~/.ssh/id_ed25519.pub
 3. Start agent and add key:
 eval "$(ssh-agent -s)" && ssh-add ~/.ssh/id_ed25519
 4. Copy public key to clipboard (macOS):
 pbcopy < ~/.ssh/id_ed25519.pub
 (If not on macOS: cat ~/.ssh/id_ed25519.pub and copy manually.)


In GitHub Enterprise Webpage: 
Settings → SSH and GPG keys → New SSH key Paste the public key and save-

In Terminal: 
Test and clone:
 ssh -T  git@github.hpe.com
 git clone  git@github.hpe.co:ying-wang/VMM-Config-Composer.git ~/Downloads/VMM-Config-Composer

Open the Downloads/VMM-Config-Composer folder and double-click index.html to open it in Chrome or Edge.


 For HPE users only — concise steps:

   1. Generate a key and add it to the agent:
   ssh-keygen -t ed25519 -C "your@hpe.com" -f ~/.ssh/id_ed25519
  chmod 600 ~/.ssh/id_ed25519 && chmod 644 ~/.ssh/id_ed25519.pub
  eval "$(ssh-agent -s)" && ssh-add ~/.ssh/id_ed25519
   2. Copy the public key:
   pbcopy < ~/.ssh/id_ed25519.pub
  (If not on macOS: cat ~/.ssh/id_ed25519.pub and copy manually.)
   3. Add the key in GitHub Enterprise:
   Settings → SSH and GPG keys → New SSH key — paste the public key and save.
   4. Test & clone:
   ssh -T git@github.hpe.com
  git clone git@github.hpe.com:ying-wang/VMM-Config-Composer.git ~/Downloads/VMM-Config-Composer
   5. Open the app:
   Open ~/Downloads/VMM-Config-Composer/index.html in Chrome or Edge.

  Important: paste only the public key (.pub). If you get “permission denied” ask the repo owner to grant you access.


# making tool

现在针对一下机型进行跑通性测试：
VPTX10003
VMX
VEX
VPTX10016
VPTX10008
VQFX3500
VMX10008
VPTX5000
VSPIRENT
LINUX
 


/Users/annaw/Annabelle/AI/Cursor/vmm-composer/fixtures/single



vptx5k+vptx10008+spirent  

VQFX3500 VPTX5000 VEX VPTX10003



OK:
VMX 单机箱
VPTX10003 vbrackla 单机箱
VQFX3500 单机箱
VPTX10008(vScapa) 单机箱+ Dual RE
VPTX10008(vScapa) 单机箱+ Single RE
VPTX10016(vScapa) 单机箱 + Dual RE
VPTX10016(vScapa) 单机箱 + Single RE
VEX 单机箱

VPTX10008 + VMX10008 +VMX 
VQFX3500 + VPTX10016
VQFX3500 + VMX
VPTX10008 + VMX10008
VPTX10016 + VMX10008
VPTX10008 +VMX
VMX10008 +VMX
VPTX10008 + VMX10008 +VMX +vspirent 
VPTX10008+VMX +vspirent 
VPTX10008 +vspirent 
VMX10008 +VMX +vspirent 
VMX10008 +vspirent
VPTX10016 + VMX10008 +VMX +vspirent
VPTX10016 + VMX10008 +vspirent
VPTX10016 + vspirent
VPTX10016 + VMX
VPTX10016 + VMX10008 +VMX+VEX +vspirent 
VPTX10016 + VMX10008 + VEX 
VMX10008 +VMX+VEX +vspirent 
VMX10008 +VEX +vspirent
VEX +vspirent
VMX+vspirent
VMX+VEX +vspirent
VPTX10008 + VMX+VEX +vspirent
VPTX10008 +VEX +vspirent
VPTX10008+VMX+VEX
VMX+VEX
vmx-vscapa16-vmx10008-vex-linux-freebsd
vscapa16-vmx10008-vex-linux-freebsd
vscapa16-vmx10008-linux-freebsd
vmx10008-linux-freebsd
vscapa16-linux-freebsd
vmx10008-linux
vmx10008-freebbsd
vscapa16-freebsd
vscapa16-linux
vscapa-freebsd
vmx-vptx10008-vmx10008-vex-linux-freebsd-vspirent
vptx10008-vmx10008-vex-linux-freebsd-vspirent
vmx10008-vex-linux-freebsd-vspirent
vex-linux-freebsd-vspirent
VPTX10016 + VEX
vptx10008 + linux + freebsd + vspirent
vex + linux + freebsd + vspirent 
vmx + linux + freebsd + vspirent	
多台 ptx_chas16
多台 vBrackla




tomorrow do vmm configure 
 VPTX10003+VQFX3500+VPTX10016+vmx10008+VMX+VEX+linux+vspirent <<<



vBrackla + VMX10008 可再 ±VMX10008、±经典 VMX、±VEX、±VQFX 逐步加
vBrackla + VQFX3500
vBrackla + VMX10008
VQFX3500 + PTX16+vmx
VQFX3500 + PTX16 + Linux + FreeBSD（± Spirent）











