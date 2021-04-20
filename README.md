# Test linux rfc4821 TCP Path MTU Discovery

Main entry point is [test-tcp-mtu-probing](test-tcp-mtu-probing).

It has minimal requirements: bash iperf iproute2 ethtool

Kernel requires network namespaces, iptables, veth

## Parameters

The test program can be configured through environment variables:

* ``TIMEOUT_SEC``: Test timeout (default 30)
* ``ICMP_BLACKHOLE``: Drop all icmps in order to test tcp-level ptmu discovery (default 1 because that's the point)
* ``TCP_MTU_PROBING``: Override net.ipv4.tcp_mtu_probing (default 1 so feature is turned on when loss is detected)
* ``RUN_TCPDUMP``: Also capture (on both client and server, default 0)
* ``EXTRA_CLIENT_CMD``: Extra shell command to run inside client namespace (via sh -c)
* ``EXTRA_SERVER_CMD``: Extra shell command to run inside server namespace (via sh -c)
* ``IPERF_CLIENT_EXTRA_ARGS``: Extra args for iperf --client (will be split by shell)
* ``IPERF_SERVER_EXTRA_ARGS``: Extra args for iperf --server (will be split by shell)

This parameter list is incomplete.
