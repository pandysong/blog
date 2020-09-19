---
date: 2020-03-28
title: Linux Kernel, regmap
weight: 10
---

# Why `regmap`

In Linux kernel, if we will need to access I2C or SPI, we usually use the
e.g. `struct i2c_client` to access the bus directly. Common target to access
the I2C device is to access the registers, for either reading or writing.

`regmap` basically adds another abstraction layer on top of `struct i2c_client`
to make it easy to read or write registers.

User could just use `struct i2c_client` and `struct regmap_config` to construct
a `struct regmap`, and then use that to read and write register (or only update
masked bits in the register using `regmap_update_bits`). All the common logic
is configured in this regmap_config.

Following section is from:
https://opensourceforu.com/2017/01/regmap-reducing-redundancy-linux-code/

# struct regmap_config

This is a per device configuration structure used by the regmap sub-system to
talk to the device. It is defined by driver code, and contains all the
information related to the registers of the device. Descriptions of its
important fields are listed below.

- reg_bits: This is the number of bits in the registers of the device, e.g., in
case of 1 byte registers it will be set to the value 8.

- val_bits: This is the number of bits in the value that will be set in the
  device register.  writeable_reg: This is a user-defined function written in
driver code which is called whenever a register is to be written. Whenever the
driver calls the regmap sub-system to write to a register, this driver function
is called; it will return ‘false’ if this register is not writeable and the
write operation will return an error to the driver. This is a ‘per register’
write operation callback and is optional.  wr_table: If the driver does not
provide the writeable_reg callback, then wr_table is checked by regmap before
doing the write operation. If the register address lies in the range provided
by the wr_table, then the write operation is performed. This is also optional,
and the driver can omit its definition and can set it to NULL.

- readable_reg: This is a user defined function written in driver code, which
  is called whenever a register is to be read. Whenever the driver calls the
regmap sub-system to read a register, this driver function is called to ensure
the register is readable. The driver function will return ‘false’ if this
register is not readable and the read operation will return an error to the
driver. This is a ‘per register’ read operation callback and is optional.

- rd_table: If a driver does not provide a readable_reg callback, then the
  rd_table is checked by regmap before doing the read operation. If the
register address lies in the range provided by rd_table, then the read
operation is performed. This is also optional, and the driver can omit its
definition and can set it to NULL volatile_reg: This is a user defined function
written in driver code, which is called whenever a register is written or read
through the cache. Whenever a driver reads or writes a register through the
regmap cache, this function is called first, and if it returns ‘false’ only
then is the cache method used; else, the registers are written or read
directly, since the register is volatile and caching is not to be used. This is
a ‘per register’ operation callback and is optional.

- volatile_table: If a driver does not provide a volatile_reg callback, then
  the volatile_table is checked by regmap to see if the register is volatile or
not. If the register address lies in the range provided by the volatile_table
then the cache operation is not used. This is also optional, and the driver can
omit its definition and can set it to NULL.

- lock: This is a user defined callback written in driver code, which is called
  before starting any read or write operation. The function should take a lock
and return it. This is an optional function—if it is not provided, regmap will
provide its own locking mechanism.

- unlock: This is user defined callback written in driver code for unlocking
  the lock, which is created by the lock routine. This is optional and, if it’s
not provided, will be replaced by the regmap internal locking mechanism.

- lock_arg: This is the parameter that is passed to the lock and unlock
  callback routine.  fast_io: regmap internally uses mutex to lock and unlock,
if a custom lock and unlock mechanism is not provided. If the driver wants
regmap to use the spinlock, then fast_io should be set to ‘true’; else, regmap
will use the mutex based lock.

- max_register: Whenever any read or write operation is to be performed, regmap
  checks whether the register address is less than max_register first, and only
if it is, is the operation performed. max_register is ignored if it is set to
0.

read_flag_mask: Normally, in SPI or I2C, a write or read will have the highest
bit set in the top byte to differentiate write and read operations. This mask
is set in the higher byte of the register value.  write_flag_mask: This mask is
also set in the higher byte of the register value.
