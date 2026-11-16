# Engineering Notes: Real-Time State Invalidation & Resilient Reconnection Protocols

**Date:** November 16, 2026  
**Category:** Distributed Client Systems / Real-Time Synchronization  

## Overview
Architecting reliable real-time event streaming pipelines across WebSockets and SSE, focusing on silent socket failure detection, reconnection storm mitigation, and sequence-based state catch-up.

### Key Learnings & Implementations
1. **Heartbeat & Zombie Socket Detection**:
   - Establishing bidirectional heartbeat pings (`ping/pong` frames) on a 15-second cadence with a strict timeout threshold to identify silent half-open TCP connections.
   - Tagging active sessions with monotonic epoch counters to prevent delayed callbacks from superseded connection attempts.
2. **Jittered Truncated Exponential Backoff**:
   - Mitigating the thundering herd problem during backend failovers by applying exponential backoff paired with full jitter (`t = random(0, min(max_interval, base_interval * 2^retry))`).
   - Clamping retry caps to avoid draining device radio/battery while maintaining timely recovery.
3. **Sequence-Based Delta Catch-Up**:
   - Emitting monotonic sequence numbers on all broadcast server events, allowing the client to maintain a persistent `last_received_seq` checkpoint.
   - Initiating a targeted delta request upon reconnection to fetch missed event ranges, avoiding redundant full state re-fetches and eliminating UI flickering.
