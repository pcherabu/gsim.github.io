# pcherabu.github.io
GSim's Project Page

Proposal: https://github.com/pcherabu/gsim.github.io/blob/master/GSim.pdf

Checkpoint: https://github.com/pcherabu/gsim.github.io/blob/master/15418_projectcheckpoint_gsim.pdf

_________________________________________________________________________________________________________________________________
GSim Development Schedule:
________________________________________________________________________

Proposed Schedule (On a weekly basis):
Week 1: Develop Front-end parser that can take in compiled CUDA code

Week 2: Built up a basic front-end

Week 3: Develop Scheduler and start on the execution model of a single-core

Week 4: Develop L1 caches and refill/miss/hit flows (this requires going back to the front-end)

Week 5: Draft a rough L2/GPU Device Memory model that takes a lot of characteristics of our L1 cache model to provide an inaccurate yet complete memory subsystem.

------------------------------------------------------------------------
Project Schedule (On a bi-weekly basis):
Week 1.0: Explored the NVidia Cuda compiler flow with NVCC. We primarily focused this half of the week in taking a look at the CUDA binary utilities that exist for Nvidia's developer community. The focus revolved around cuobjdump (operating from x86 environments only).

Week 1.5: Found a couple of bottlenecks in our work flow; this stemmed from our realization that we can't simply use cuobjdump and .cudin files as inputs to our simulator. Primarily, we have realized that we would require to make our own "input files" for GSIM. We have termed these files as "gimage files". "gimage" files are "input image files" that is formed from a separate tool chain before entering the simulator. The toolchain (which we are developing in the second half of week one) will take in .cudin files in order to parse through elf sections of the .cudein files) and generate gimage files that consists of low-level nvdisassembly language which will our parser will be able to parse and input information of each instruction to "FInst" formats (FInst is a Fetched Instruction that is created for instruction that is "fetched" and parsed).

Week 2.0: Week 1 was full of problems understanding the nvcc toolchain in order to get it to work and develop our own toolchains from interstage nvcc compiler outputs. Hence, we decided to split the work and bring the Proposed Week 3 schedule of developing a scheduler and the interface between our front end and back end of our simulator. Thus, work for generating gimages from .cudin files and developing the scheduler are going on silmuntaneously. We have further developed on FInst, then MInst class which will have a pointer to FInst and more fields of data (this is now the instruction packet after decoding the fetched instruction from FInst front-end processing). Scheduler interfaces take in MInst and convert that to AInst (which are allocated Instructions). AInst work and scheduling "blocks" and "warps" are taking place concurrently.

Week 2.5: Scheduler work presses on, still coding up MInst and AInst classes and their member functionality. Core design of a single core is saved to Week 3 as initially proposed by the schedule.

***Hit a major problem with the front end CUDA code toolchain support we're trying to develop: nvcc takes in .cudin files and wraps them into fatbinaries. Fatbinaries are later linked to the original .cpp source code of the base with nvlink to combine into the fatbinary. How do we get past or ignore the nvlink stage? We can potentially stay near .cudin files but then we won't be able to scheduler the cuda code... meaning the simulator won't actually execute it unless we come with a hack.***

Week 3.0 (Current week... Checkpoing Update): Scheduler work is on hold and the entire team is on the front end problem. This changes the scope of GSim, maybe GSim approaches this in a hacky way? Have the user manually give options to scheduling out kernels (so we don't need the source code of this)? ***The current design plan is to have our front-end be a wrapper around nvbit (because it does most of the work we’re trying to do from scratch).***

***PLANNED WORK***
Week 3.5: Integrate NVBit and SASSI to have GSIM "replace" its front end with options that will call different dynamic instrumentation tools integrated with these PINs. Then, it will produce traces that we wrap into a tasteful mix of MInst and AInst instruction packets we have existing. We then input them to our scheduler. This way, our simulator is a weird mix of a functional trace-driven simulator with cycle-approximate models. To clarify, our simulator will now generate its own trace files by taking in normal .cu code files and giving it into NVBit (for example) which will output certain instrution information to use as our front-end. We take these packets and then simulate then on our back-end (scheduler and gpu cores) that calculates AI in a cycle-approximate back end model.

Week 4.0: Finish writing all dynamic instrumentation tools on NVBit to generate instruction traces. Start up scheduler work and wrap it up (at this point we're done with all credit checks on core resources and shedule out blocks to each core).

Week 4.5: Start on the core model (the actual simulation of the instruction itself). Start developing cache and memory subsystem model. We lose accuracy here, but have fixed latency arrays to determine communciation costs.

Week 5.0: Come back on the proposed schedule and draft a rough model of shared memory at high level caches and the device memory. This means simulating coherency manager and introducing invalidation protocols on our L1 cache. This final week will be the wrap up on calculating AI.

Week 5.5: Finish up the CM and shared L2 (have ideal dynamic memory if time does not permit). Introduce flow statistics here to calculate AI and histogram support via maps. This will slowdown simulation times tremendously, but get the statistics we need on every run of the code.
