
两个HPE GitHub上有账户：
1. **github.hpe.com** — `ying-wang` 账户（企业GitHub）<<<企业敏感数据
2. **github.com** — `ying-wang_hpeprod` 账户（公共GitHub上的HPE账户） <<< 搭配copilot最方便
私人GitHub:
annabelle16252 icallthisdestiny@hotmail.com



tsNP yB"data"
User Name: ying-wang_hpeprod
license HPE & training:
https://hpe.sharepoint.com/sites/RAISE/SitePages/GitHub-Integration.aspx?web=1





# Juniper Troubleshooting Default Workflow
Issue-First Rule (Mandatory)

- Always start from the customer issue statement and requested impact.
- Build a focused scope first: issue time window, affected protocol/process/FRU/interface, and expected behavior.
- Analyze issue-related logs first. Do not expand into unrelated logs unless issue evidence is insufficient.
- If adding extra log domains (for example, kernel/pfe/rpd/chassisd), explain why they are relevant to the customer issue.
- Keep findings explicitly tied to the issue question (for example: "Does this explain the observed flap/down/shutdown?").

Auto-Execute Rule (Mandatory)

- For log analysis, RCA, mcp server lookup, or read-only SSH log access: proceed immediately, no confirmation needed. Do NOT ask "shall I proceed?" or permission to SSH.
- Exception: actions that modify device configuration, send commits, restart processes, or write files require explicit user confirmation.

Log Handling Rule (Mandatory)

- Preference order: plain-text log file > already-extracted directory > compressed archive.
- Only decompress if no pre-extracted equivalent is available.

Evidence-Based Analysis Rule (Mandatory)

- All RCA conclusions MUST be directly supported by evidence extracted from logs. Do NOT hypothesize, guess, or infer beyond what logs explicitly show.
- When presenting a root cause, include:
  1. Exact log line(s) or timestamp(s) that prove the finding
  2. Device/daemon name where the evidence appears (e.g., rpd[pid], kernel, lacpd)
  3. Temporal correlation (if A happens at HH:MM:SS and B at HH:MM:SS+Δ, show the delta)
- If logs are insufficient to reach a conclusion, explicitly state "Log evidence is missing for [hypothesis]" and request specific commands from customer.
- Do NOT present assumptions as facts. Use phrases like:
  - Proven by logs: "Log shows X at timestamp T, proving Z"
  - Cannot confirm: "No log evidence for Y; customer data incomplete"
  - Requires validation: "Hypothesis H needs output from [command] to confirm"
- When logs show multiple possible causes, evaluate each against available evidence and explain why one is more likely (with evidence) than others.

BGP Flap Causality Guardrails (Mandatory)

- For every RPD_BGP_NEIGHBOR_STATE_CHANGED event where state changes from Established to Idle, you MUST produce a correlated event window table for T-10s to T+2s.
- In each correlated window, explicitly scan and report results for these trigger signatures (zero hits must still be reported):
  1. ifName xe-  link state traps
  2. RPD_ISIS_ADJDOWN
  3. BGP_IO_ERROR_CLOSE_SESSION
  4. BGP down reason tokens: HoldTime, TransportError, RecvNotify
  5. reachability failures: sendto: No route to host, SNMPD_SEND_FAILURE
  6. LACP_INTF_MUX_STATE_CHANGED
- Root-cause reasoning MUST follow this strict layer order:
  1. Physical/Link (interface up/down, optics, LACP)
  2. IGP/Transport (ISIS/OSPF/LDP reachability)
  3. BGP session behavior
  4. EVPN/service impact
- Do NOT infer causality from correlation alone. Any "X caused Y" statement is valid only if time(X) <= time(Y) and no earlier lower-layer trigger contradicts it.
- A candidate root cause cannot be finalized from a single flap sample. Confirm the same trigger pattern across repeated flap events before declaring primary cause.
- Every final BGP RCA statement MUST include one of: Proven by logs / Cannot confirm / Requires validation

Standard Request Format Trigger

- When a message begins with "Juniper case analysis request" and contains fields "Customer Issue:", "log path:", and "Task:", treat it as a full RCA task and immediately begin execution without any confirmation prompt.
- "Customer Issue:" → issue statement and impact
- "log path:" → remote or local path to access logs (apply JTAC Remote Log Path Rule)
- "Task:" numbered items → specific analysis objectives to answer in the final output
- Execute all steps (KB search, mediawiki if AFT platform, log access, RCA) in sequence and return a single structured report.

When the user provides Juniper logs, alarms, or asks for RCA:

1. Anchor on customer issue first.
- Restate issue in one line (symptom + impact + time window).
- Analyze issue-related logs only; expand scope only if evidence is insufficient.

2. Use kb-annaw-mcp as a knowledge-guided RCA engine.
- Search using log-extracted keywords (protocol, daemon, error code, platform).
- Use KB to decide which logs/hypotheses to prioritize and which commands to request from customer.
- If no KB match: state that clearly, then provide best-effort hypothesis and a minimal confirm/deny command list.

2.1 Use mediawiki-mcp for PFE/hardware/AFT-platform issues (see 2.2). Skip for pure control-plane issues unless KB or logs point to a hardware angle.
- Treat as internal guidance only; combine with KB + log evidence.
- Use search snippets if full text is unavailable; label confidence accordingly.

2.2 AFT FPC mandatory mediawiki trigger.
- When the FPC/LC or platform is any of the following, ALWAYS query mediawiki first:
  - MPC10E (Ferrari, 1.5T), MPC11E (RedBull, 4T), MX10K-LC9600 (Alfa Romeo, 9.6T), JNP10K-LC4800, MX304
- Debugging pages:
  - LC9600: Scapa_Debugging, Scapa_Traffic_Debugging, Scapa_Fabric_Debugging
  - MPC10E/11E: Trio Debugging, Trio L2 Debugging, Trio/ZT Firewall Debugging
  - VMX ZT: VMX-ZT Debugging
  - PPE traps/crash: PPE Traps and AutoTTRACE
  - Scale/performance: Scale and Performance Debugging
  - Host path/PacketIO: PacketIO, PacketIO Debug
  - Index of all: Debug_Guides
  - CLI syntax: Command_References
- On AFT FPCs prefer wiki-sourced commands over Trio-era commands (AFT uses aftman, AftServer, CDA, PacketIO daemons).

3. Use junos-mcp for focused validation only. Run the minimal command set that directly confirms or denies the suspected cause.

4. Provide RCA and validation steps.
- Most likely RCA based on log + KB evidence, validation commands, fix/mitigation options.

5. Final output order: KB match summary → RCA → Risk and next steps.

JTAC Remote Log Path Rule

- Log paths like /volume/CSdata/annaw/case/<case-id> are remote. SSH to annaw@10.104.8.126 before concluding a path is missing.
- Use read-only commands only (ls, find, head, tail, grep).
- Do not store plaintext credentials in files or long-term memory.

