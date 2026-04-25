/volume/case_2026/2026-0422-685188/core-SN-SINPL1-BO406-fpc0-smpc.elf.1.tgz
/volume/CSdata/annaw/case/2026-0422-685188
rsi-varlog /volume/CSdata/annaw/case/2026-0422-685188

Hostname: SN-SINPL1-BO406
Model: mx2008
Junos: 23.2R2-S2.5

-rw-r--r--  1 root  wheel  393527368 Apr 22 00:44 /var/crash/core-SN-SINPL1-BO406-fpc0-smpc.elf.1.tgz

FPC 0            REV 56   750-063414   CAGZ6242          MPC9E 3D





root@SN-SINPL1-BO406-re0> show log messages | match CMERROR
Apr 22 00:37:46.570 2026  SN-SINPL1-BO406 : %PFE-5: fpc0 CMError: /fpc/0/pfe/0/cm/0/EA[2:0]/2/EACHIP_CMERROR_HMCIF_RX_LINK_INT_REG_HMC_FATAL_ERR (0x2400c1), scope: pfe, category: functional, severity: major, module: EA[2:0], type: HMCIF RX link int reg HMC fatal err, oc_category: default 
Apr 22 00:37:46.571 2026  SN-SINPL1-BO406 : %PFE-5: fpc0 Performing action get-state for error /fpc/0/pfe/0/cm/0/EA[2:0]/2/EACHIP_CMERROR_HMCIF_RX_LINK_INT_REG_HMC_FATAL_ERR (0x2400c1) in module: EA[2:0] with scope: pfe category: functional level: major, oc_category: default 


EACHIP_CMERROR_HMCIF_TX_INT_REG_FO_DEALLOC_IDX_HALF_CHUNK_ERR


# KB
KB78500 [MX] FPC Major Errors - EACHIP_CMERROR_HMCIF_RX_LINK_INT_REG_HMC_FATAL_ERR (0x2400c1)

# PR
https://gnats.juniper.net/web/default/1882070#audit_tab
https://gnats.juniper.net/web/default/1803916#audit_tab

# coredump

Root Cause

SMIC(0/0) 的 panic pre-processing 路径里触发了二次 panic。
Periodic 线程在崩溃清理阶段执行 stout_crash_laser_off()，该函数内部调用 smic_sleep_ms(50)，最终走到 thread_sleep()。但此时线程已经处在 suspend is disallowed 的上下文里，禁止阻塞/睡眠，所以触发：

panic("Thread %s attempted to sleep when suspend is disallowed\n")
随后进入：
vpanic() -> posix_interface_prepare_to_panic() -> posix_interface_abort() -> abort() -> SIGABRT
因此，这个 coredump 的直接死因不是普通非法内存访问，而是 panic path 中调用了 sleep，导致二次 abort。