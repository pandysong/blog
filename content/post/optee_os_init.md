---
date: "2019-08-27"
title: Compile the optee_os
weight: 10
---

# Get the source code

```
git clone https://github.com/OP-TEE/optee_os.git
```

## How to Compile

The code must be compiled with cross-compiler, I have an quad-A7 SDK from
Rockchip.

```
CROSS_COMPILE=path/to/gcc/linux-x86/arm/arm-eabi-4.8/bin/arm-eabi- make
```

The default PLATFORM would be vexpress, in order to select a platform:

```
PLATFORM=rockchip CROSS_COMPILE=path/to/gcc/linux-x86/arm/arm-eabi-4.8/bin/arm-eabi- make
```

A complete list of platform could be found in `core/arch/arm`.

# PSCI (Power State Coordination Interface)

One of the secure firmware is to start/stop/suspend additional cores. PSCI is
the interface defined by Linux and implemented by underlying vendor specific
firmware to power on or power off or suspend the additional cores

Following document describe the detailed standard interface:

http://infocenter.arm.com/help/topic/com.arm.doc.den0022d/Power_State_Coordination_Interface_PDD_v1_1_DEN0022D.pdf

For example following figure shows that the command 0x84000000 could be used to
fetch the psci implementation version in the firmware:

![psci_version](/img/psci_version_api.png)

And following command defines the command 0x84000001 to suspend a CPU for
ARM32:

![psci_version](/img/psci_suspend.png)

Which is corresponding to the DTS configuration for the psci driver in arm

```
    psci {
        compatible = "arm,psci";
        cpu_off = <0x84000002>;
        cpu_on = <0x84000003>;
        cpu_suspend = <0x84000001>;
        method = "smc";
    };
```

The driver code in `arch/arm/kernel/psci.c` looks like:
```
  void __init psci_init(void)
  {

    ...

      if (!of_property_read_u32(np, "cpu_suspend", &id)) {
          psci_function_id[PSCI_FN_CPU_SUSPEND] = id;
          psci_ops.cpu_suspend = psci_cpu_suspend;
      }
  ...
  }
```

The ops is saved in `psci_function_id`. When needed, we use it to suspend the
CPU:

```
static int psci_cpu_suspend(struct psci_power_state state,
                  unsigned long entry_point)
  {
      int err;
      u32 fn, power_state;

      fn = psci_function_id[PSCI_FN_CPU_SUSPEND];
      power_state = psci_power_state_pack(state);
      err = invoke_psci_fn(fn, power_state, entry_point, 0);
      return psci_to_linux_errno(err);
  }
```

This function is saved in global variables `psci_ops` and used in `psci_smp.c`
by the function psci_boot_secondary which is exposed by `psci_smp_ops`.

```
struct smp_operations __initdata psci_smp_ops = {
      .smp_boot_secondary = psci_boot_secondary,
  #ifdef CONFIG_HOTPLUG_CPU
      .cpu_die        = psci_cpu_die,
      .cpu_kill       = psci_cpu_kill,
  #endif
  };

```


```
__cpu_up -> boot_secondary -> smp_ops.smp_boot_secondary(cpu, idle);
-> rockchip_boot_secondary
```
