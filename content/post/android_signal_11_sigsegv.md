---
date: 2020-04-04
title: Fatal signal 11 (SIGSEGV), code 1 (SEGV_MAPERR), fault addr 0x0 in tid 1720 (android.hardwar), pid 1720 (android.hardwar) 
weight: 10
---

# Introduction

This document recalls how a crash dump is traced back in source code.

# Issue

```
01-18 09:00:20.529  1707  1707 F libc    : Fatal signal 11 (SIGSEGV), code 1 (SEGV_MAPERR), fault addr 0x0 in tid 1707 (android.hardwar), pid 1707 (android.hardwar)
01-18 09:00:20.562  1710  1710 I crash_dump64: obtaining output fd from tombstoned, type: kDebuggerdTombstone
01-18 09:00:20.563   482   482 I /system/bin/tombstoned: received crash request for pid 1707
01-18 09:00:20.563  1710  1710 I crash_dump64: performing dump of process 1707 (target tid = 1707)
01-18 09:00:20.565  1710  1710 F DEBUG   : *** *** *** *** *** *** *** *** *** *** *** *** *** *** *** ***
01-18 09:00:20.566  1710  1710 F DEBUG   : Build fingerprint: 'rockchip/rk3368/rk3368:9/PQ2A.190205.003/pandy03311514:userdebug/test-keys'
01-18 09:00:20.566  1710  1710 F DEBUG   : Revision: '0'
01-18 09:00:20.566  1710  1710 F DEBUG   : ABI: 'arm64'
01-18 09:00:20.566  1710  1710 F DEBUG   : pid: 1707, tid: 1707, name: android.hardwar  >>> /vendor/bin/hw/android.hardware.power@1.0-service <<<
01-18 09:00:20.566  1710  1710 F DEBUG   : signal 11 (SIGSEGV), code 1 (SEGV_MAPERR), fault addr 0x0
01-18 09:00:20.566  1710  1710 F DEBUG   : Cause: null pointer dereference
01-18 09:00:20.566  1710  1710 F DEBUG   :     x0  0000000000000000  x1  0000000000000000  x2  000000000000000a  x3  0000000000000003
01-18 09:00:20.566  1710  1710 F DEBUG   :     x4  0000000000000000  x5  8080000000000000  x6  feff4b4047716476  x7  7f7f7f7f7f7f7f7f
01-18 09:00:20.566  1710  1710 F DEBUG   :     x8  0101010101010101  x9  0000000000000000  x10 0000000000000020  x11 0000007639199e8b
01-18 09:00:20.566  1710  1710 F DEBUG   :     x12 0000007639199e8c  x13 0000000000000000  x14 0000000000000000  x15 ffffffffffffffff
01-18 09:00:20.566  1710  1710 F DEBUG   :     x16 0000007639c310e0  x17 0000007639b60290  x18 0000000000000000  x19 000000000000000a
01-18 09:00:20.566  1710  1710 F DEBUG   :     x20 0000000000000000  x21 00000076391b9140  x22 0000007feb3d0db8  x23 0000007639215380
01-18 09:00:20.566  1710  1710 F DEBUG   :     x24 000000763a0dd5e0  x25 0000007639215380  x26 0000007639215380  x27 0000007feb3d0d89
01-18 09:00:20.566  1710  1710 F DEBUG   :     x28 0000000000000001  x29 0000007feb3d0a50
01-18 09:00:20.567  1710  1710 F DEBUG   :     sp  0000007feb3d0a30  lr  0000007639b89b44  pc  0000007639b602a0
01-18 09:00:20.576  1710  1710 F DEBUG   : 
01-18 09:00:20.576  1710  1710 F DEBUG   : backtrace:
01-18 09:00:20.576  1710  1710 F DEBUG   :     #00 pc 000000000001e2a0  /system/lib64/libc.so (strlen+16)
01-18 09:00:20.576  1710  1710 F DEBUG   :     #01 pc 0000000000047b40  /system/lib64/libc.so (__strcpy_chk+32)
01-18 09:00:20.576  1710  1710 F DEBUG   :     #02 pc 00000000000009cc  /vendor/lib64/hw/power.rk3368.so (rk_power_init+180)
01-18 09:00:20.576  1710  1710 F DEBUG   :     #03 pc 0000000000003348  /vendor/lib64/hw/android.hardware.power@1.0-impl.so (HIDL_FETCH_IPower+240)
01-18 09:00:20.576  1710  1710 F DEBUG   :     #04 pc 0000000000030980  /system/lib64/vndk-sp-28/libhidltransport.so (_ZZN7android8hardware25PassthroughServiceManager3getERKNS0_11hidl_stringES4_ENKUlPvRKNSt3__112basic_stringIcNS6_11char_traitsIcEENS6_9allocatorIcEEEESE_E_clES5_SE_SE_+96)
01-18 09:00:20.577  1710  1710 F DEBUG   :     #05 pc 000000000002cda4  /system/lib64/vndk-sp-28/libhidltransport.so (android::hardware::PassthroughServiceManager::openLibs(std::__1::basic_string<char, std::__1::char_traits<char>, std::__1::allocator<char>> const&, std::__1::function<bool (void*, std::__1::basic_string<char, std::__1::char_traits<char>, std::__1::allocator<char>> const&, std::__1::basic_string<char, std::__1::char_traits<char>, std::__1::allocator<char>> const&)> const&)+788)
01-18 09:00:20.577  1710  1710 F DEBUG   :     #06 pc 000000000002f120  /system/lib64/vndk-sp-28/libhidltransport.so (android::hardware::PassthroughServiceManager::get(android::hardware::hidl_string const&, android::hardware::hidl_string const&)+96)
01-18 09:00:20.577  1710  1710 F DEBUG   :     #07 pc 000000000002dfc4  /system/lib64/vndk-sp-28/libhidltransport.so (android::hardware::details::getRawServiceInternal(std::__1::basic_string<char, std::__1::char_traits<char>, std::__1::allocator<char>> const&, std::__1::basic_string<char, std::__1::char_traits<char>, std::__1::allocator<char>> const&, bool, bool)+1364)
01-18 09:00:20.577  1710  1710 F DEBUG   :     #08 pc 000000000000f70c  /system/lib64/vndk-28/android.hardware.power@1.0.so (_ZN7android8hardware7details18getServiceInternalINS0_5power4V1_09BpHwPowerENS4_6IPowerEvvEENS_2spIT0_EERKNSt3__112basic_stringIcNSA_11char_traitsIcEENSA_9allocatorIcEEEEbb+204)
01-18 09:00:20.577  1710  1710 F DEBUG   :     #09 pc 0000000000000b3c  /vendor/bin/hw/android.hardware.power@1.0-service
01-18 09:00:20.577  1710  1710 F DEBUG   :     #10 pc 0000000000000aa4  /vendor/bin/hw/android.hardware.power@1.0-service
01-18 09:00:20.577  1710  1710 F DEBUG   :     #11 pc 00000000000ab570  /system/lib64/libc.so (__libc_init+88)
```

# backtrace the code

Using the little tool in https://github.com/pandysong/crashdump:

Following code backtrace is got:


```
/proc/self/cwd/bionic/libc/arch-arm64/generic/bionic/strlen.S:102:  pc 000000000001e2a0  /system/lib64/libc.so (strlen+16)
bionic/libc/bionic/fortify.cpp:475:  pc 0000000000047b40  /system/lib64/libc.so (__strcpy_chk+32)
out/soong/.intermediates/bionic/libc/libc.llndk/android_arm64_armv8-a_cortex-a53_vendor_shared/gen/include/bits/fortify/string.h:93:  pc 00000000000009cc  /vendor/lib64/hw/power.rk3368.so (rk_power_init+180)
hardware/interfaces/power/1.0/default/Power.cpp:34:  pc 0000000000003348  /vendor/lib64/hw/android.hardware.power@1.0-impl.so (HIDL_FETCH_IPower+240)
system/libhidl/transport/ServiceManagement.cpp:380:  pc 0000000000030980  /system/lib64/vndk-sp-28/libhidltransport.so (_ZZN7android8hardware25PassthroughServiceManager3getERKNS0_11hidl_stringES4_ENKUlPvRKNSt3__112basic_stringIcNS6_11char_traitsIcEENS6_9allocatorIcEEEESE_E_clES5_SE_SE_+96)
external/libcxx/include/functional:1916:  pc 000000000002cda4  /system/lib64/vndk-sp-28/libhidltransport.so (android::hardware::PassthroughServiceManager::openLibs(std::__1::basic_string<char, std::__1::char_traits<char>, std::__1::allocator<char>> const&, std::__1::function<bool (void*, std::__1::basic_string<char, std::__1::char_traits<char>, std::__1::allocator<char>> const&, std::__1::basic_string<char, std::__1::char_traits<char>, std::__1::allocator<char>> const&)> const&)+788)
system/libhidl/transport/ServiceManagement.cpp:368:  pc 000000000002f120  /system/lib64/vndk-sp-28/libhidltransport.so (android::hardware::PassthroughServiceManager::get(android::hardware::hidl_string const&, android::hardware::hidl_string const&)+96)
system/libhidl/transport/ServiceManagement.cpp:750:  pc 000000000002dfc4  /system/lib64/vndk-sp-28/libhidltransport.so (android::hardware::details::getRawServiceInternal(std::__1::basic_string<char, std::__1::char_traits<char>, std::__1::allocator<char>> const&, std::__1::basic_string<char, std::__1::char_traits<char>, std::__1::allocator<char>> const&, bool, bool)+1364)
system/libhidl/transport/include/hidl/HidlTransportSupport.h:156:  pc 000000000000f70c  /system/lib64/vndk-28/android.hardware.power@1.0.so (_ZN7android8hardware7details18getServiceInternalINS0_5power4V1_09BpHwPowerENS4_6IPowerEvvEENS_2spIT0_EERKNSt3__112basic_stringIcNSA_11char_traitsIcEENSA_9allocatorIcEEEEbb+204)
bionic/libc/bionic/libc_init_dynamic.cpp:129:  pc 00000000000ab570  /system/lib64/libc.so (__libc_init+88)
```
