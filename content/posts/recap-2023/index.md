+++
author = "Strato Team"
title = 'Recap of 2023'
date = 2024-01-13
description = "A recap of what 2023 has been for us, what we worked on in the past year and what we're currently up to."
+++

# Happy new year, everyone! :tada:

We wanted to give you a recap of what 2023 has been for us, what we worked on in the past year and what we're currently up to.

## What did we work on in 2023?

As you know by now, our main focus was `texman`, a rewrite of the **texture manager**.  
[TheASV](https://github.com/TheASVigilante) initially took on the variant system for textures that Mark had been working on and polished it, making **guest texture aliasing** emulation compliant with the Vulkan spec. This change alone improved compatibility with **Mali** GPUs a lot.  
A new **texture sync system** was introduced along with a `UsageTracker` that does sequencing on currently existing textures and syncs between them on demand.  
In the meantime he also made some fixes to **queries** (a way for games to retrieve information about the processing of a sequence of graphics commands), implemented **MSAA** rendering and made some improvements to the **blit engine**.  
All of the above sum up to major rendering **accuracy improvements**, but none of the changes so far bring performance improvements.

Outside of texman, we worked on a few more things.  
Most notably, we fixed the **JNI bug** with our signal handler causing crashes when games used vibrations or the software keyboard.  
We also implemented an **asynchronous logger** replacing our old synchronous one, and we re-enabled debug and verbose logging levels as a result. They were previously disabled for being too slow, but we will now be able to use them in our testing and debugging.  
Lastly, we've added support for **importing firmwares**, this should increase compatibility with games that use firmware assets.

There's lots of other smaller changes that we didn't think would fit into this small progress report, but rest assured you'll be able to read them all in our release changelog, once it drops.  
Some of them were a result of the work of **contributors** over the past months, thanks to [PabloG2](https://github.com/PabloG02), [dima-xd](https://github.com/dima-xd) and [Adrien](https://github.com/QuackingCanary) for the help!

## What is the team up to now?

[TheASV](https://github.com/TheASVigilante) is currently working on getting back the **performance** we lost because of the rendering accuracy improvements. Texman was a big rewrite and we had to focus on correctness first before diving into **optimisations**.

[Lynx](https://github.com/lynxnb) has been busy with **JIT** for a while, and he still is. Work on JIT has been interrupted and resumed many times in the last year for various reasons, but it's now a prority for the team.  
We're going to use **Dynarmic** to run **32-bit games**, which are the last few games not booting on Strato yet. With Strato being hardwired for NCE, a lot of work has gone into reworking core components of the emulator to support JIT execution, while ensuring not to introduce performance regressions for NCE execution.  
There's still lots left to be done but we're close to getting the first 32-bit game to actually run.

That's all for now. Thank you for your ongoing support and stay tuned for future updates!
