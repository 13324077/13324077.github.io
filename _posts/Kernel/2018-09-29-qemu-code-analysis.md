---
layout: post
title: qemu源码分析
date: 2018-09-29 21:32:30 +08:00
category:
    - Linux内核
keywords: Qemu
tags:
    - Qemu
---

type_init()定义在module.h头文件中

```c
#define block_init(function) module_init(function, MODULE_INIT_BLOCK)
#define opts_init(function) module_init(function, MODULE_INIT_OPTS)
#define type_init(function) module_init(function, MODULE_INIT_QOM)
#define trace_init(function) module_init(function, MODULE_INIT_TRACE)
```

而module_init()也定义在module.h头文件中

```c
#ifdef BUILD_DSO
void DSO_STAMP_FUN(void);
/* This is a dummy symbol to identify a loaded DSO as a QEMU module, so we can
 * distinguish "version mismatch" from "not a QEMU module", when the stamp
 * check fails during module loading */
void qemu_module_dummy(void);

#define module_init(function, type)                                         \
static void __attribute__((constructor)) do_qemu_init_ ## function(void)    \
{                                                                           \
    register_dso_module_init(function, type);                               \
}
#else
/* This should not be used directly.  Use block_init etc. instead.  */
#define module_init(function, type)                                         \
static void __attribute__((constructor)) do_qemu_init_ ## function(void)    \
{                                                                           \
    register_module_init(function, type);                                   \
}
#endif
```

对于qemu模拟的arm处理器中，通过如下进行相关初始化

```c
static void arm_cpu_register_types(void)
{
    const ARMCPUInfo *info = arm_cpus;

    type_register_static(&arm_cpu_type_info);
    type_register_static(&idau_interface_type_info);

    while (info->name) {
        cpu_register(info);
        info++;
    }

#ifdef CONFIG_KVM
    type_register_static(&host_arm_cpu_type_info);
#endif
}

type_init(arm_cpu_register_types)
```

其中arm_cpu_type_info的定义如下:

```c
static const TypeInfo arm_cpu_type_info = {
    .name = TYPE_ARM_CPU,
    .parent = TYPE_CPU,
    .instance_size = sizeof(ARMCPU),
    .instance_init = arm_cpu_initfn,
    .instance_post_init = arm_cpu_post_init,
    .instance_finalize = arm_cpu_finalizefn,
    .abstract = true,
    .class_size = sizeof(ARMCPUClass),
    .class_init = arm_cpu_class_init,
};
```

其中的class_init调用关系如下图所示, 在vl.c文件中的main函数中调用object_new进行初始化，最终调用class_init。

![qemu-class-init-call-relation.png](/images/qemu/qemu-class-init-call-relation.png)
