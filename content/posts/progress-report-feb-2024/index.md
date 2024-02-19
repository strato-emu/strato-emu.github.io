+++
author = "Strato Team"
title = "Progress Report February 2024"
date = 2024-02-19
description = "Progress report as of February 2024"
toc = true
+++

Hey everyone! We're back with your monthly dose of Strato news. This month, [TheASV](https://github.com/TheASVigilante) continued his work on optimising GPU, while [Lynx](https://github.com/lynxnb) got most of JIT done.

## GPU changes

### Texture recreation
On the GPU side of things, we introduced a new **texture recreation system**. `texman` needs to recreate host texture objects after particular GPU commands are received, either because render target dimensions are lazily provided by the 3D engine, or because we exclude some image usage flags from Vulkan image objects until they become required for correct emulation, for performance reasons. The recreation system that we originally wrote for `texman` had many issues and deadlocked often, the new system fixes all of that.

### GPU channels
We changed GPU channels to share a **single command executor** instead of having one per channel. Having one per channel was pointless because they were never actually allowed to run concurrently, thus taking up resources for no benefit. Together with other minor changes to the executor, this helped **stability** massively and fixed all presentation issues we were having during internal testing.

### Fence cycles
We also changed how **fence cycles** and **chaining** works. Fence cycles are an abstraction over the Vulkan concept of a `VkFence` introduced in Skyline, used to let the CPU know when a particular GPU command has completed execution or submission. The main reason fence cycles exist is that they allow objects to attach their **lifetime** to them, so that they can be **destroyed safely** once they're not in use anymore.
Previously, fence cycles were intrusively chained and recursively iterated through on every wait. This meant that any circular dependency would cause infinite recursion, or that the locking could needlessly take up extreme amounts of time. We perform chaining via an external structure now, which solves all of the aforementioned issues and improves performance in certain games due to **much less waiting**.

Testing of the above is still ongoing, but we might be able to give you some sneak peeks really soon, stay tuned for that!

## JIT changes
As for JIT, most of the core components of the emulator have been modified to work with the JIT and are now ready to run 32-bit games.

### 32-bit processes and scheduler
**Thread setup** has been reworked to include a code path for **32-bit** JIT processes. Because of our scheduler design, guest code cannot directly call SVCs, so we need to halt the JIT and return from guest code when an SVC has to be executed, or the preemption timer fires.

### `SvcContext`
Before the end of last year we introduced a **common register context** for SVCs.
With 32-bit processes using the same SVCs as 64-bit ones but with different thread contexts, we needed a way to **decouple register access** from the SVC implementations to avoid duplicating them.
We came up with `SvcContext`, a structure containing only the minimal amount of registers needed by SVCs. 
SVCs were then refactored to operate on that structure, resulting in **minimal code changes** for supporting 32-bit SVCs.

## Release date
We are happy to announce that Strato is set to release in the **first half of 2024**! We'll share all the details on the exact date soon, as we get closer to the release, so stay tuned for that.

That's all for this month. Thank you for your ongoing support and see you soon!
