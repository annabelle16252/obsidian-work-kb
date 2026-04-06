# Document

LRM & JT AUTOMATION FOR VMM
https://junipernetworks.sharepoint.com/:p:/r/teams/Junos/eng-tools/test-tools/projects/testtech-vj/_layouts/15/Doc.aspx?sourcedoc=%7B412ED15E-595C-42E1-B5D7-5261B21C266A%7D&file=Training%20-%20JT%20Automation%20with%20Virtual%20JUNOS%20(VMX).pptx&action=edit&mobileredirect=true&DefaultItemOpen=1

Params Enhancement:
https://junipernetworks.sharepoint.com/:p:/r/teams/RBU/test/platform/PFE%20Platform%20Test%20Engineering/_layouts/15/Doc.aspx?sourcedoc=%7BE9FB0E89-BC9D-439E-973F-F8EA86906592%7D&file=Params%20Enhancements.pptx&action=edit&mobileredirect=true&DefaultItemOpen=1

```
          r1 ───────r3 ───────r5
     /                             \
ce1 ────────────────────────────────────ce2
     \                             /
          r2 ───────r4 ───────r6
```

# SG Groups
+:(toby-shell-users): ALL
+:(jdi-all): ALL
you can subscribe using the MyGroups portal: (https://mygroups.juniper.net/)

# Run Pbuilder
1.Server login
Please switch to less loaded servers like ttsv-shell015,ttsv-shell001,ttsv-shell014,ttsv-shell007
annaw@q-pod13-vmm:~$ ssh annaw@ttsv-shell002
annaw@ttsv-shell001's password: JNPR PW
Connection closed by 10.108.196.21 port 22
annaw@q-pod13-vmm:~$ 

2.run 'pbuilder -t' in ENV:
ttsv-shell002:~$ export PATH="$PATH:/volume/labtools/bin"
ttsv-shell002:~$ which pbuilder
/volume/labtools/bin/pbuilder

export VM_DEPLOY=1
export VM_DEPLOY_MODELS="R0=vscapa":"R1=vbrackla"
export DT_DOMAIN=q-pod30-vmm
export DT_AVAILABLE=1h
export DYNAMIC_TOPOLOGY=1
export DT_FORCE=1
export VMX_PHASE=3
export DT_DEBUG=1
export VM_GROUP=vmm-default
export VM_DEPLOY_TIMEOUT=1200
pbuilder -t vSCAPA_2RE-vBrackla_2RE.params

annaw@ttsv-shell002:~$ ls 
vSCAPA_2RE-vBrackla_2RE.params.vmmconfig <<<


# Other commands

```
/volume/labtools/bin/pbuilder -t test.params
                         OR
/volume/labtools/bin/pbuilder -t test.params



/volume/labtools/bin/res sh -w user annaw     =======>>(once topology created find your VM)

Showing reservation

 id         bind_id  unit                  model     by_userid  start_at              end_at               
 322553652  None     tt-annaw-17269449-vm  vscapa    annaw      2026-03-26 16:17 PDT  2026-03-26 17:17 PDT 
 322553668  None     tt-annaw-17269451-vm  vbrackla  annaw      2026-03-26 16:17 PDT  2026-03-26 17:17 PDT 

 
/volume/labtools/bin/params-info VM_NAME  =====> (to see your vms parameter, like IP, console Etc)

annaw@ttsv-shell002:~$ /volume/labtools/bin/params-info tt-annaw-17269449-vm 
Current information on tt-annaw-17269449-vm:
  Resource state................ACTIVE
  Manufacturer..................juniper
  Model.........................vscapa
  Testbed.......................global
  Domain........................q-pod30-vmm
  Re_Type.......................RE-VSCAPA
VM Details:
  VM Type.......................Non-Persistent
  VM Console....................q-pod-88o.englab.juniper.net 15048
  VM Host.......................q-pod-88o.englab.juniper.net
  VM UUID.......................41a62392-2969-11f1-b222-ef894600adf3
  VMM Account...................annaw:*****
  VM Owner......................annaw
  Host tt-annaw-17269449-vm:re1
    Osname......................JunOS 
    Ip..........................10.54.169.128/19
    Ipv6........................2620:103:c008:4e:5604:aff:fe36:a980/64
    Loopip......................128.54.163.52
    Loopipv6....................abcd::128:54:163:52
    Conip.......................q-pod-88o.englab.juniper.net 15049
  Host tt-annaw-17269449-vm:re0
    Osname......................JunOS 
    Ip..........................10.54.163.52/19
    Ipv6........................2620:103:c008:4e:5604:aff:fe36:a334/64
    Loopip......................128.54.163.52
    Loopipv6....................abcd::128:54:163:52
    Conip.......................q-pod-88o.englab.juniper.net 15048
 et-0/0/1                  tt-annaw-17269451-vm et-1/0/0:0                             
 et-0/0/3                  tt-annaw-17269451-vm et-1/0/0:1                             
 lo0                                                                                   
 re0:mgmt-0                                                                            
 re1:mgmt-0   


annaw@ttsv-shell002:~$ ssh root@10.54.163.52 <<< device login by VMM root pw

/volume/labtools/bin/lrm-fs update destroy_vmm_vm   VM_NAME (To destroy topology)

```


# params file

```

annaw@ttsv-shell002:~$ cat vSCAPA_2RE-vBrackla_2RE.params 
R0 {
    system {
        x-my-is-vm "true";
        vm-deploy-model "vscapa";
    }
    hosts {
           count 2;
          }
    interfaces {
        connect10 {
        }
        connect11 {
        }
    }
}
R1 {
    system {
        x-my-is-vm "true";
        vm-deploy-model "vbrackla";
        vm-deploy-mpc-type "default=B10GZX";
    }
# [ VBRACKLA ] does not support dual RE.
    interfaces {
        connect10 {
        }
        connect11 {
        }
    }
}


```


# Tool
you can create it graphically with this tools 
https://inception.juniper.net/gte

# Support models
Supported models :[SUPPORTEDPIC, VMX, VBGPROUTESERVER, VMX-BASE, VMX86, VMX960, VMX480, VMX240, VMX2010C, VMX2008, VMX2010, VMX2020, VMX10008, VMX10004, VMX304, VMX301, VMX9608, VJPG, VJX, VEX, VEX9214, VEX9208, VEX9204, VJUNOS-MX, VJUNOS-EX, VJUNOS-EVO, CJUNOS-EVO, VJUNOS-EVO-BX, CJUNOSEVO-CLAB, VJUNOS-EVO-BX, VPTX, EVOVPTX, EVOVPTX1SNG, EVOVPTX2SNG, EVOVPTX3SNG, EVOVPTX4SNG, EVOVPTX1HEN, EVOVPTX2HEN, EVOVPTX3HEN, EVOVPTX4HEN, EVOVPTX1POL, EVOVPTX2POL, EVOVPTX3POL, EVOVPTX4POL, EVOVPTX1OME, EVOVPTX2OME, EVOVPTX3OME, EVOVPTX4OME, VSCAPA, VLIGENPLUSEVO, VSCAPA16, VBOWMORE, VKX, PTX-VSONIC, VQFX10, VQFXTVP, VQFX12, VVALE, VPTX10008, VQFX5200, VBRACKLA, VSRXHA, VSRXVIO, OLIVE, VRR, LINUX, LIGEN, B4-CONTROLLER, NORTHSTAR, APSTRA, ODL, PARAGON-K8-CONTROLLER, PARAGON-WORKER, PARAGON-MASTER, BCS-JUMPHOST, BCS-NODE, UI-NODE-LINUX, ATOM-MASTER, ATOM-WORKER, PAA-CONTROL-CENTER, PAA-TEST-AGENT, IXAPPSERVER, VSPIRENT, VIXIA, IXVMCHASSISSERVER, IXVM, IXNEWVM, FREEBSD, VEVO, VATTELLA, VSAPPHIRE, VARDBEG, VBALERION, VBALERION-2BX, VWHISTLER, VSPECTROLITE, UBUNTU, CRPD, CBNG, CSRX, NCPTX, NCPTX-SETUP, CPTX, CIXIA-KNE, CPTX-SETUP, CIXIA-DOCKER, CMGD, CPTX, CMX, VOCTOMORE, PCMGD, SDV44, VNFX, ]