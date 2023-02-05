---
date: 2022-06-07
title: How to resolve mds_stores high CPU issue on Mac
weight: 10
---

# mds_stores -> spotlight, the mac search engine

For those who mainly work on limitted number of documents, it does not make
much trouble, as it index fast enough. But for those who managed a lot of files
and has a lot of messages on instant messenger, the process mds_stores may
cause high cpu on you mac.


# how to resolve

I want it to index on the application names and normal documents, but not on
the source code or vast messages on the instant messenger.

Check what mds is busy for:

```
sudo fs_usage -w -f filesys mds_stores
sudo fs_usage -w -f filesys mds
sudo fs_usage -w -f filesys mds_shared
```

You may find it is busy on reading some path. If the path is not what you want
to index in, put the path on privacy path on following configuration:

```
system preference -> spotlight -> privacy
```

# check the cpu temperature

```
sudo powermetrics --samplers smc |grep -i "CPU die temperature"
```
