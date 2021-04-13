# Test linux rfc4821 TCP Path MTU Discovery

Main entry point is [test-tcp-mtu-probing](test-tcp-mtu-probing).

It has minimal requirements: bash iperf iproute2 ethtool

Kernel requires network namespaces, iptables, veth

## Parameters

The test program can be configured through environment variables:

* ``TIMEOUT_SEC``: Test timeout (default 30)
* ``RUN_TCPDUMP``: Also capture (on both client and server, default 0)
* ``EXTRA_CLIENT_CMD``: Extra shell command to run inside client namespace (via sh -c)
* ``EXTRA_SERVER_CMD``: Extra shell command to run inside server namespace (via sh -c)

This paramter list is incomplete.
