---
date: 2021-04-18
title: Access SMB server from Mac Terminal
weight: 10
---


# could not ls or copy file to the server


```
/Volumes/releases ls
ls: .: Operation not permitted
```

We however could copy files to server on Mac `finder` application.

# Solution


```

I had the same problem and it was fixed by granting »Full disk access« to
Terminal in System preferences -> Security & Privacy -> Privacy and restarting
Terminal.

Click the lock at the bottom left of the window in case the options are greyed
out.

```

