# Ubuntu Service Configuration

Paste the following into /etc/init/audiogram.conf:

```
# Your script information
description "Audiogram Runner"
author      "Nathan Lawrence"

# Describe events for your script
start on startup
stop on shutdown

# Respawn settings
# Without this stanza, a job that exits quietly transitions into the stop/waiti$
respawn
# respawn limit COUNT INTERVAL
respawn limit 10 5

# Run your script!
# If your script returns string "EXIT", the daemon will stop itself.
script
    [ $(exec /home/[[[username here (ubuntu on aws)]]]/audiogram/bin/server) = 'EXIT' ] && ( stop; exit 1; )
end script
```

And, if you want to be connected through the default HTTP port (80), make sure you change that in bin/server.

Then, to start or stop the script, just run `sudo service audiogram start` or `sudo service audiogram stop`. After you've made changes to the package, make sure you run `npm postinstall` in the audiogram folder before re-enabling the service.