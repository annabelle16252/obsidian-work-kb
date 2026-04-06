https://github.hpe.com/michael-tucker1/rsi-explorer
https://github.hpe.com/ying-wang/rsi-explorer-JTAC


我的pr：
内容是 config mode、matcher、pager 等 JTAC 功能，?,

```
uvx --from git+ssh://git@github.hpe.com/ying-wang/rsi-explorer-JTAC.git@v1.4.3 \
  rsi-cli --remote-host 10.104.8.126 --remote-user annaw \
 "/volume/CSdata/annaw/case/2026-0312-632154/20260312-xn-paoeq1-bo101-re0-rsi.txt"


uvx --from git+ssh://git@github.hpe.com/ying-wang/rsi-explorer-JTAC.git@v1.4.3 \
  rsi-cli --remote-host <your server IP > --remote-user <User Name >  \
"< /volume/rsi path > "


uvx --from git+ssh://git@github.hpe.com/ying-wang/rsi-explorer-JTAC.git@v1.4.4 \
  rsi-cli --remote-host 10.104.8.126 --remote-user annaw \
  --download-dir ~/rsi-cache \
  "/volume/CSdata/annaw/case/2026-0312-632154/20260312-xn-paoeq1-bo101-re0-rsi.txt"
  
  
uvx --from git+ssh://git@github.hpe.com/ying-wang/rsi-explorer-JTAC.git@v1.4.4 \
  rsi-cli --remote-host 10.104.8.126 --remote-user annaw \
  --download-dir ~/rsi-cache \
  "< /volume/rsi path >"  
  
  
Download RSI will be save in your local directory '~/rsi-cache'. You can use this same command everytime to access the RSI and download is only for one time.
```
When --remote-host is used, the RSI file is copied to a randomly named rsi-cli-* temporary directory under the system temp location and automatically deleted when the CLI process exits.

# Github SSH

```
This setup is required to securely authenticate your local machine with GitHub Enterprise over SSH so you can clone, fetch, and run internal tools that use git+ssh without HTTPS.


GitHub Enterprise SSH Setup (6-Step SOP for macOS)

1.Generate an SSH key on your local machine: 
ssh-keygen -t ed25519 -C "your.work.email@hpe.com" (press Enter for defaults).
2.Start ssh-agent and load your key: 
eval "$(ssh-agent -s)" && ssh-add ~/.ssh/id_ed25519.
3.Copy your public key to clipboard: 
pbcopy < ~/.ssh/id_ed25519.pub 
4.In GitHub Enterprise, go to Settings -> SSH and GPG keys -> New SSH key, paste the key, and save.
5.Verify SSH auth to GHE: 
ssh -T git@github.hpe.com (type yes on first host key prompt).
6.Verify repo access: 
git ls-remote git@github.hpe.com:ying-wang/rsi-explorer-JTAC.git (if refs are returned, you are ready).

```


  
# Issue
-configuration mode: how to go to that level :/
```
P1256869_RW@MTHKGCOLO-RR01# show | match 101.234.3.141
            neighbor 101.234.3.141;
```


uvx --from git+ssh://git@github.hpe.com/ying-wang/rsi-explorer-JTAC.git@v1.4.2 rsi-cli "/volume/CSdata/annaw/case/2026-0312-632154/20260312-xn-paoeq1-bo101-re0-rsi.txt"

"/Users/annaw/Annabelle/Case/my_rsi/2026/2026-0122-049996_20260119-mx-bd-05-07-rsi.log"
/volume/CSdata/annaw/case/2026-0312-632154/20260312-xn-paoeq1-bo101-re0-rsi.txt

# Function
```
> show interfaces extensive ae255
> show interfaces extensive| match 'Physical link is Down'
> no-forwarding
> key_bindings & ? (keep Tab) completer
> configuration mode/edit 
> : -> ---(more)---
> 
> regress@PE1_RE> show chassis h          
Possible completions:
  hardware             Show installed hardware components
  hss                  Show SerDes information

```

# Cloned to my local MAC

```
/volume/case_2026/
/volume/CSdata/annaw/case/

1.Copy RSI from JTAC Tool:

jtac_rsi "/volume/CSdata/annaw/case/2026-0310-628387/RSI_MX-BP-02-06_2026-03-10_140901.txt"

2.
> rsi-cli /Users/annaw/Annabelle/Case/my_rsi/2026/2026-0312-632154_20260312-xn-paoeq1-bo101-re0-rsi.txt

```rsi-explorer

# Push local change to github
```
最好每次让AI给你push，因为不能都push，只push更改的文件

cd /Users/annaw/Annabelle/AI/Cursor/202501/rsi-explorer
git status
git add <具体文件或路径>
git commit -m "your message"
git push origin main

```

# Pre-install requirement

```
1. Prerequisites (macOS)
Open Terminal.
Make sure Apple Command Line Tools are installed:
xcode-select --install
If you see “Command line tools are already installed”, you’re good.

2. Install uv (gives you uvx)
curl -LsSf https://astral.sh/uv/install.sh | sh
After it finishes, add ~/.local/bin to your PATH (temporarily for this terminal):
export PATH="$HOME/.local/bin:$PATH"
Verify:
uv --versionuvx --version
You should see version numbers, not “command not found”.
(If this works, you can later add
export PATH="$HOME/.local/bin:$PATH"
to your ~/.zshrc so it’s permanent.)

3. Set up SSH key for GitHub Enterprise
Generate an SSH key (if you don’t already have one):
ssh-keygen -t ed25519 -C "your_corp_email@your_company.com"
Press Enter to accept the default file location.
Optionally set a passphrase.
Start the SSH agent and add your key:
eval "$(ssh-agent -s)"ssh-add ~/.ssh/id_ed25519
Copy the public key:
cat ~/.ssh/id_ed25519.pub
Copy the whole line that starts with ssh-ed25519.
Add the key to GitHub Enterprise:
Open https://github.hpe.com in your browser.
Go to Settings → SSH and GPG keys → New SSH key.
Paste the public key, give it a name, and save.
Test SSH connection:
ssh -T git@github.hpe.com
You should see a “successfully authenticated” message (or similar).


```
