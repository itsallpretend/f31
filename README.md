# F31 — unsupported and unsafe for production use

F31 is an outdated experiment. Its network and system changes have not been
validated for safe operation or reliable restoration. **Do not run these scripts
on a production, engagement, or other important host.** The project makes no
verified OPSEC, anonymity, or detection-evasion guarantees.

The scripts can modify system identity, NetworkManager configuration, time
synchronization, kernel network settings, firewall rules, the network interface
MAC address, and traffic shaping. The reset script assumes default values and
does not reliably restore the previous host state. Partial failures may leave
changes behind or interrupt network access.

## Remediation priorities

Before considering any operational use, the project needs:

1. A defined, supported operating-system and network configuration scope.
2. Safe argument validation and explicit confirmation of the target host and
   interface.
3. Per-run capture of the actual prior state, with reliable restoration of only
   changes made by that run.
4. Failure handling that reports incomplete changes accurately, plus
   idempotent apply and restore behavior.
5. Automated tests for parsing, state capture, failure paths, and restoration.
6. Independent review and controlled environment testing. These are not yet
   complete.

## Validation approach

Start with non-privileged tests and command mocks for argument handling,
state-management logic, and simulated failures. Then test system changes and
restoration in disposable local virtual machines with snapshots and an isolated
virtual network. Verify the baseline is restored after normal completion and
after interruption or failure. Keep the environment disconnected from external
targets.

Only after those checks pass should a controlled Red/Blue exercise be
considered, and then only against systems owned by the testing team or
explicitly authorized for that exercise. Measure both operational impact and
defensive telemetry; do not treat a lack of alerts as proof of stealth or
OPSEC. Restore snapshots and confirm cleanup before ending the exercise.

For hosted environments, a local VM is the better first step because the test
needs privileged, host-level network configuration and easy snapshots. Railway
is a poor fit for that kind of system-level testing. A disposable Hetzner VM
could be considered later if local testing is insufficient, but only in a
private, isolated network with no route to third-party targets; network changes
can also sever remote access. Do not put provider credentials in this
repository.

Until the remediation and validation work is complete, the scripts are retained
for reference only and are not recommended for execution.
