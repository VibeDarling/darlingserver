# Inactive ARM64 hardware debug state

Compile `arm64-inactive-debug-state.c` as an ARM64 Darwin executable using the
configured Darling SDK and libSystem, with assertions enabled. Run it against
the matching private darlingserver:

```sh
darling shell /Volumes/SystemRoot/absolute/path/to/arm64-inactive-debug-state
```

Before the change, the first `thread_get_state(ARM_DEBUG_STATE64)` assertion
fails with `KERN_NOT_SUPPORTED`. The corrected server returns the full zero
state and accepts restoring it. The test also checks short get/set buffers,
rejects each nonzero byte independently, and verifies rejected updates leave
the state inactive.

Active hardware debugging remains unsupported; accepting arbitrary register
updates would make this test fail. This does not establish full .NET exception
compatibility.
