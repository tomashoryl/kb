# Spanning-Tree Protocol

## Summary
> As a best practice, it is always recommended to manually set up each bridge's _priority_, _port priority_, and _port path cost_ to ensure proper Layer2 functionality at all times.


## STP and RSTP
One of the reasons why RSTP is faster is because of reduced possible port states.

STP port states:
- **Forwarding** - port participates in traffic forwarding and is learning MAC addresses, and is receiving BPDUs.
- **Listening** - port does not participate in traffic forwarding and is not learning MAC addresses, is receiving BPDUs.
- **Learning** - port does not participate in traffic forwarding but is learning MAC addresses.
- **Blocking** - port is blocked since it is causing loops but is receiving BPDUs.
- **Disabled** - port is disabled or inactive.

RSTP port states:
- **Forwarding** - port participates in traffic forwarding and is learning MAC addresses, is receiving BPDUs (forwarding=yes).
- **Learning** - port does not participate in traffic forwarding but is learning MAC addresses (learning=yes).
- **Discarding** - port does not participate in traffic forwarding and is not learning MAC addresses, is receiving BPDUs (forwarding=no).

> In STP ports are primarily categorized by states (e.g., Forwarding, Listening, Learning, Blocking, Disabled). Port behavior is determined dynamically based on the spanning tree algorithm but without explicitly assigning roles. The logic of forwarding or blocking traffic is derived from the calculation of Root Bridge, Root Ports, and Designated Ports, but these are considered part of the spanning tree topology rather than formalized port roles. RSTP explicitly defined port roles and introduces the concept of backup paths, which are explicitly represented through the Alternate Port and Backup Port roles. These roles did not exist in STP because STP treated blocked ports generically, without distinguishing their function as potential backups.

RSTP port roles:
- **Root Port** - port that is facing towards the root bridge and has the best (lowest cost) path to the root bridge. Only one root port is elected per bridge (except the root bridge itself).
- **Designated Port** - port that is facing away from the root bridge and forwards traffic away from the root bridge to downstream devices.
- **Alternate Port** - port that is facing towards the root bridge, but is not going to forward traffic. Port provides a backup path to the root bridge if the current root port fails.
- **Backup Port** - port that is facing away from the root bridge, but is not going to forward traffic. Port that serves as a backup for a designated port on the same segment.
- **Disabled Port** - disabled or inactive port.

In STP connectivity 


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


# STP Root Failure

## Event Detection
Every non-root switch keeps a timer on its *Root Port* based on the *Hello Time* (default 2 seconds).
- In RSTP, BPDUs are generated by every switch, not just relayed from the root as in STP.
- If a switch stops receiving BPDUs from the root (or from the designated switch on the path to the root) for *3x Hello Time* (default 6s), it assumes the path to the root is lost.

This is much faster than classic STP's *Mas Age* of 20 seconds.

## Immediate Local Actions
When the root is presumed dead:
1. The current *Root Port* on each switch is invalidated.
2. MAC addresses learnt on non-edge ports are flushed immediately.
3. The *Root Role Selection* state machine runs to pick a new root bridge.

## New Root Election Process
The switches compare bridge IDs from the best BPDUs they still have:
- If a switch has no superior BPDU anymore, it assumes itself to be the root and starts sending its own BPDUs advertising its *Bridge ID* as the root.
- Neighbors receive these new BPDUs, compare with their own, and agree on the lowest bridge ID.
- This propagates hop-by-hop until one switch is recognized as the new root.

## Port State Transitions
For every switch after a new root is chosen:
1. *Root Port Selection* - pick the port with the best path to the new root.
2. *Alternate / Backup Ports* - ports that do not lead to the root are placed into *Discarding*.
3. *Synchronization Process*
    - Each designated port sends a *Proposal BPDU*.
    - Neighbor replies with an *Agreement* if it can put all other ports into *Discarding* to avoid loops.
    - On receiving Agreement, the proposing port goes *immediately to Forwarding*.
4. This negotiation allows RSTP to converge in a few milliseconds per hop instead of waiting 30-50 seconds.
