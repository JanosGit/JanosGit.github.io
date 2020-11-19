---
layout: post
title: About analog modeling part 1 – General thoughts
---

While I've been working a bit as audio plugin developer now, analog circuit modeling is something that has not been part of my daily job until now. Neither was the design of analog audio circuits. So this part of the OJD was quite an interesting challenge for me as well, especially with my electrical engineering background. There might be experts with far more knowledge in this field out there, but here is what I learned so far... be prepared, the upcoming post(s) might require some knowledge in digital signal processing ;)

### Which modeling approach to chose?
There are various approaches on how to model analog audio circuits with digital signal processing. They can be more or less grouped into three groups:

#### 1. Component based or "White Box" modeling

This is the most straightforward modeling approach that can be applied if you have deep knowledge on the circuit you want to model. As in theory every circuit can be fully calculated in every state if you know all parameters of the components involved, it is possible to use this approach to compute how a certain circuit would react to a certain input signal. However, everyone who ever simulated a circuit with simulation tools like e.g. Spice, knows that this can take some amount of time for medium complex circuits. Furthermore, everyone that ever tried to apply those Spice simulation results with a real world circuit will have noticed that this sometimes works better and sometimes worse. Especially complex non-linear circuits are crazily complex to compute. Bad news: Non-linearities are one of the key aspects that make analog audio hardware sound so nice... So a component-based modeling approach will always require a deep understanding of the circuit, that makes it possible to spot the parts that are crucial for the sound and to simplify parts of the circuit that won't make any noticeable difference anyway. This will likely keep the required processing power reasonably low. Furthermore you'll have to chose which level of approximation is enough for certain parts of the circuit. When it comes down to the pure math, there are different mathematical approaches on how to compute a circuit in general – my knowledge in this field is still quite limited.

#### 2. Black Box modeling

A black box modeling approach knows nothing about the schematics. You typically assume a basic block-diagram of the signal flow that originates from the knowledge about the typical structure of the device you want to model, e.g. for a distortion pedal like the OJD one could assume something like filter -> gain -> non-linear waveshaper -> filter -> gain. While this abstract approach might simplify the DSP code a lot, it opens up two questions: How to derive all the parameters for the processing blocks from the original hardware? And what level of precision (e.g. filter orders) is needed to achieve a satisfying result (or can a satisfying result be achieved this way at all?). Regarding the second part, one could start with a massive over-precise approach and reduce precision step by step until the user notices a difference. Regarding the first part, this one gets especially difficult if the device has multiple control elements. Accurately capturing one specific setting is likely to be successful – this is the approach that products like the Kemper Profiling Amp rely on. But creating a model with interactive control elements that act together just like the original hardware does means that you would have to capture parameters in hundreds of combinations of parameters. This will be a tough to impossible task.

#### 3. "Grey Box modeling" – The best of both worlds

It is possible to combine both approaches. If the schematic is known, parts of a schematic that are highly interactive and parts of a schematic that are likely to not interact with the surrounding parts can be determined. This way, a black-box-like block diagram that groups (nearly) non interactive blocks can be created. We can then have a look if these blocks are e.g. linear, time-invariant or not and chose an appropriate modeling approach for each block. With the schematics at hand, it's furthermore possible to see which controls affect which block, which will make measurements under different settings far less complex.

This is the approach, that I have chosen to model the OJD. More on that in part two!
