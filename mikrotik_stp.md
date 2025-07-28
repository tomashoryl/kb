# Spanning-Tree Protocol

## Edge Port
Setting a bridge port as an edge port will restrict it from sending BPDUs and will ignore any received BPDUs.

Set port as edge port or non-edge port, or enable edge discovery. Edge ports are connected to a LAN that has no other bridges attached. An edge port will skip the learning and the listening states in STP and will transition directly to the forwarding state, this reduces the STP initialization time. If the port is configured to discover edge port then as soon as the bridge detects a BPDU coming to an edge port, the port becomes a non-edge port. This property has no effect when `protocol-mode` is set to `none`.

- `no` - non-edge port with disabled discovery, will participate in learning and listening states in STP. It will not transition to the forwarding state until it exchanges BPDUs and reaches agreement with the connected bridge. If no BPDU is received, the port may remain in a non-forwarding state indefinitely.
- `no-discover` - non-edge port with enabled discovery, will participate in learning and listening states in STP, a port can become an edge port if no BPDU is received.
- `yes` - edge port without discovery, will transit directly to forwarding state.
- `yes-discover` - edge port with enabled discovery, will transit directly to forwarding state.
- `auto` - same as `no-discover`, but will additionally detect if a bridge port is a Wireless interface with disabled bridge-mode, such interface will be automatically set as an edge port without discovery.

## Point-to-point
Specifies if a bridge port is connected to a bridge using a point-to-point link for faster convergence in case of failure. By setting this property to `yes`, you are forcing the link to be a point-to-point link, which will skip the checking mechanism, which detects and waits for BPDUs from other devices from this single link. By setting this property to `no`, you are expecting that a link can receive BPDUs from multiple devices. By setting the property to `yes`, you are significantly improving (R/M)STP convergence time. In general, you should only set this property to `no` if it is possible that another device can be connected between a link, this is mostly relevant to Wireless mediums and Ethernet hubs. If the Ethernet link is full-duplex, `auto` enables point-to-point functionality. This property has no effect when `protocol-mode` is set to `none`.
