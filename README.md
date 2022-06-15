# Test linux rfc4821 TCP Path MTU Discovery

In theory enabling tcp_mtu_probing should make MSS increase close to the MTU
value but this depends on actual traffic conditions.

Main entry point is [test-tcp-mtu-probing](test-tcp-mtu-probing). This test
works by creating three namespaces linked by veth pairs, dropping ICMP in the
middle namespace and then running iperf between the client and server.

At the start all namespaces are set to allow the "himtu", then the middle
namespace decrease MTU to "lomtu" and waits for TCP mss to decrease. Afterwards
the MTU is increased again and mtu probing should reach an MSS close to the
himtu value.

Smaller scripts test specific scenarios:
* [test-long-writes](test-long-writes): Test with 256k writes and 256k window
* [test-tiny-writes](test-tiny-writes): Test with 8k writes and 256k window

Userspace requirements are minimal: bash iperf iperf3 iproute2 ethtool. For
debian-based distros they are listed in [apt-requirements.txt](apt-requirements.txt).

Kernel requirements are also minimal: network namespaces, iptables, veth

## Parameters

The test program can be configured through environment variables:

* ``TIMEOUT_SEC``: Test timeout (default 30)
* ``ICMP_BLACKHOLE``: Drop all icmps in order to test tcp-level ptmu discovery (default 1 because that's the point)
* ``TCP_MTU_PROBING``: Override net.ipv4.tcp_mtu_probing (default 1 so feature is turned on when loss is detected)
* ``TEST_RISE_ONLY``: If set to 1 then only test reaching himtu once. Only makes sense if TCP_MTU_PROBING is 2
* ``RUN_TCPDUMP``: Also capture (on both client and server, default 0)
* ``RUN_PING``: Also run ping once before iperf
* ``MIDDLE_DELAY``: Insert a delay in the middle using "tc netem". Both directions so RTT should increase by twice this value
* ``EXTRA_CLIENT_CMD``: Extra shell command to run inside client namespace (via sh -c)
* ``EXTRA_SERVER_CMD``: Extra shell command to run inside server namespace (via sh -c)
* ``IPERF_V3``: Use iperf3 (args are slightly different)
* ``IPERF_WINDOW``: iperf --window argument
* ``IPERF_CLIENT_EXTRA_ARGS``: Extra args for iperf --client (will be split by shell)
* ``IPERF_SERVER_EXTRA_ARGS``: Extra args for iperf --server (will be split by shell)
* ``LOG_DIR``: Directory to save outputs. Default is temporary and only permanent result is stdio.
* ``CLEAN``: If set to zero skip cleanup, leaving processes and namespaces running

This parameter list not entirely complete.
