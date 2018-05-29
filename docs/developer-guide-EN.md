<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->


- [Capture, replay and restore](#capture-replay-and-restore)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Capture, replay and restore

[`TransmittableThreadLocal.Transmitter`](../main/java/com/alibaba/ttl/TransmittableThreadLocal.java#L201) to capture all `TransmittableThreadLocal` of current thread and replay them in any other thread，there are following methods：

- `capture`: capture all `TTL` values in current thread
- `replay`: replay the captured `TTL` values in current thread, and return the backup `TTL` values before replay
- `restore`: restore `TTL` values before replay

Sample code：

```java
// ===========================================================================
// Thread A
// ===========================================================================

TransmittableThreadLocal<String> parent = new TransmittableThreadLocal<String>();
parent.set("value-set-in-parent");

// capture all TTL values in current thread
final Object captured = TransmittableThreadLocal.Transmitter.capture();

// ===========================================================================
// Thread B
// ===========================================================================

// replay the captured TTL values in current thread, and return the backup TTL values before replay
final Object backup = TransmittableThreadLocal.Transmitter.replay(captured);
try {
    // Your biz code, you can get the TransmittableThreadLocal value from here
    String value = parent.get();
} finally {
    // restore TTL values before replay
    TransmittableThreadLocal.Transmitter.restore(backup);
}
```