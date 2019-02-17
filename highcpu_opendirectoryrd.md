# High cpu due to opendirectoryrd

I found several links where people complaint about high cpu of the process "opendirectoryrd" in mac,
which causes system busy and macbook becomes very hot.

Here is the how I resolve MY problem.

## Find the problem in system log

I found following log repated many times in the system log:
```
com.apple.xpc.launchd[1] (com.avast.uninstall): Service only ran for 5 seconds. Pushing respawn out by 5 seconds.
```

How to open the system log?

Open console.app and click tab *sytem.log*

You may also monitor the log by following command line:

```
tail -f /var/log/system.log
```

## Analysis

It looks the the service is launched again and again, which keeps opendirectoryrd very busy.
I am not sure why it is is uninstalling, but there is no avast in the sytem.
It is possible the previously uninstallation was not clean.

## How to resolve

So let's try to uninstall it agian.

Download the installation program (the uninstallation program is included) from avast.
When you click it, you could choose to uninstall the current one in the system.

After uninstallation, reboot the mac and my mac comes back to *cool* state.


