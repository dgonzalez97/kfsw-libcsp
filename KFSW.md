# K-FSW changes

Fork of `libcsp/libcsp`. K-FSW pins a revision of the `kfsw` branch in its
`west.yml`, so the pin cannot move under a release.

| Change | Files | Upstream |
| --- | --- | --- |
| Stamp the source address on loopback sends | `src/csp_io.c` | not submitted |

`csp_send_direct()` short-circuits packets addressed to the local node straight
to `csp_if_lo` without passing through `send_packet()`, which is what applies
the outgoing interface address. The packet keeps `src = 0`, so the peer answers
node 0 and the reply leaves through the default route: `csp ping <own address>`
returns -3 and an RDP connection to the local address is refused. The fix runs
the loopback send through `send_packet()` like every other interface.

Rules for this fork:

- One commit per change, each with a reason in the message.
- Rebase onto upstream `develop`; do not merge upstream into `kfsw`.
- Keep this file current. When it lists nothing, the fork is only a stable pin.
