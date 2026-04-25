# EVN
```
export VM_DEPLOY=1
export VM_DEPLOY_MODELS="R0=vscapa":"R1=vqfx10"
export DT_DOMAIN=q-pod30-vmm
export DT_AVAILABLE=1h
export DYNAMIC_TOPOLOGY=1
export DT_FORCE=1
export VMX_PHASE=3
export DT_DEBUG=1
export VM_GROUP=vmm-default
export VM_DEPLOY_TIMEOUT=1200
pbuilder -t 

```


# VSCAPA2RE-VMX10008
```

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
        vm-deploy-model "vmx10008";
        vm-deploy-mpc-type "0=ALFAROMEO";
    }
    interfaces {
        connect10 {
        }
        connect11 {
        }
    }
}


```

# GLO  directory
```
/homes/akroy

```



# VMX960-EA
```
R2 {
    system {
        vm-deploy-model "vmx960";
        vm-deploy-mpc-type "0=EA10G";
    }
    interfaces {
        connect11 {
        }
        connect0 {
        }
    }
}


```

# vscapa-vqfx
```
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
         vm-deploy-model "vqfx10";
        vm-deploy-name "suman-1vqfx";
           }

      interfaces {
        connect10 {
        }
        connect11 {
        }
   }
 }
```

# vspirent-vixia
```
r0 {
    system {
        x-my-is-vm "true";
        vm-deploy-model "vspirent";
    }
    interfaces {
        connect0 {
        }
    }
}
r1 {
    system {
        x-my-is-vm "true";
        vm-deploy-model "ixvm";
    }
    interfaces {
        connect0 {
        }
    }
}
```

# vex-vex
```
r0 {
    system {
        x-my-is-vm "true";
        vm-deploy-model "vex9214";
    }
    interfaces {
        connect0 {
        }
    }
}
r1 {
    system {
        x-my-is-vm "true";
        vm-deploy-model "vex9214";
    }
    interfaces {
        connect0 {
        }
    }
}
```

# linux-freebsd
```
buntu-20
r0 {
    system {
        x-my-is-vm "true";
        vm-deploy-model "Linux";
        vm-deploy-nic-type "vio";
        vm-deploy-ram "8192";
        vm-deploy-ncpu "8";
    }
    interfaces {
        connect0 {
        }
    }
}

Freebsd
r2 {
    system {
        x-my-is-vm "true";
        vm-deploy-model "FreeBSD";
    }
    interfaces {
        connect1 {
        }
    }
}
```

# vqfx-vqfx
```
r0 {
    system {
        x-my-is-vm "true";
        vm-deploy-model "vqfx10";
    }
    interfaces {
        connect0 {
        }
    }
}
r1 {
    system {
        x-my-is-vm "true";
        vm-deploy-model "vqfx10";
    }
    interfaces {
        connect0 {
        }
    }
}
```

# vixia-vmx
```
RT0 {
    system {
        vm-deploy-model "ixvm";
    }
    interfaces {
        rt0-r1 {
        }
    }
}
R1 {
    system {
        vm-deploy-model "vmx";
    }
    interfaces {
        rt0-r1 {
        }
    }
}
```

# vBrackla
```
R1 {
    system {
        x-my-is-vm "true";
        vm-deploy-model "vbrackla";
        vm-deploy-mpc-type "default=B10GZX";
    }
    interfaces {
        connect10 {
        }
        connect11 {
        }
    }
}
```

# VMX10008
```
R1 {
    system {
        vm-deploy-model "vmx10008";
        vm-deploy-mpc-type "0=ALFAROMEO";
    }
    interfaces {
        connect10 {
        }
        connect1 {
        }
    }
}
```

# VIXIA
https://trollwiki.englab.juniper.net/mediawiki/index.php/Vixia-Linux-APP-Server
```
APP0 {
    system {
        vm-deploy-model "ixappserver";
        vm-deploy-nic-type "vio";
        vm-deploy-ncpu "12";
        vm-deploy-ram "8192";
    }
    interfaces {
        # connect0 { }
    }
}

RT0 {
    system {
        x-my-is-vm "true";
        vm-deploy-model "vixia";
        vm-deploy-nic-type "vio";
        vm-deploy-ncpu "4";
        vm-deploy-ram "4096";
        x-no-connect 1;
        x-ixia-pkg "IXIA";
        fv-ixia-license-server "q-pod-ixlic1.englab.juniper.net";
        fv-ixia-license-type "tier1";
        fv-ixia-appserver-username "admin";
        fv-ixia-appserver-password "admin";
        fv-ixia-appserver-port "443";
        fv-ixia-appserver "UserName01LnxAppSvr.englab.juniper.net";
    }
    interfaces {
       connect11 {
       }
    }
}
R0 {
    system {
        vm-deploy-model "vmx960";
        vm-deploy-mpc-type "0=EA10G";
    }
    interfaces {
        connect11 {
        }
    }
}



export DT_DOMAIN=q-pod30-vmm
export VM_GROUP=vmm-default
export VM_DEPLOY_NAME=APP0=UserName01LnxAppSvr
export DT_AVAILABLE=4h
export VM_DEPLOY_TIMEOUT=1200
export VM_DEPLOY=1
export DYNAMIC_TOPOLOGY=1
export DT_FORCE=1
export DT_DEBUG=1
export VMX_PHASE=3
export VM_DEPLOY_MODELS="R0=vmx960:APP0=ixappserver:RT0=VIXIA"
export VM_IMAGES="vmx=/vmm/data/base_disks/default_images/default_image_vmx_phase3.img":"ixappserver=/vmm/data/base_disks/ixia/ixia-vm93-as.qcow2":"VIXIA=/vmm/data/base_disks/ixia/ixia-vm93-ch.qcow2"
export IXVM_LICENSE_SERVER=q-pod-ixlic1.englab.juniper.net
pbuilder -t vixia-vmx.params



```
