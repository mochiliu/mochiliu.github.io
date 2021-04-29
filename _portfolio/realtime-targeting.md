---
title: "Real-time tracking and stimulus targeting"
excerpt: ""
header:
  image: /assets/images/worm-touch/whole_field_of_view.gif
  teaser: /assets/images/worm-touch/whole_field_of_view.gif
tags:
  - animal behavior
  - computer vision
  - real-time
  - multi-thread optimization
  - particle tracking
  - LabVIEW
  - DLP projection
  - optogenetics
---

While highly informative to our understanding of worm touch circuitry, the my [previous work](https://mochiliu.github.io/portfolio/worm-behavior/) has several limitations that my current research efforts seek to overcome: (1) the light stimulus used is only a single color, which prevents multiplexing excitatory and inhibitory stimulation in the same experiment; (2) the stimulus intensity is shared for entire plate, which means we cannot study how the touch stimulus affects the head or the tail separately; and (3) stimulation was agnostic to the animal’s behavior, which makes it challenging to gather stimulus reponse during rare behaviors. So I designed an instrument that can probe worm behavioral response to spatially coherent inhibitory and excitatory optogenetic stimuli, with real-time behavior segmentation and stimulus targeting for many animals in parallel.

<figure>
  <center><img src="/assets/images/realtime/Experimental_Design_API.png" style="width:50%"></center>
  <figcaption>Cartoon layout of the Real-Time Multimodal Optogenetic Assay System. A projector is used for targeted optogenetic stimulus delivery based on real-time tracking of worms on an agar plate.</figcaption>
</figure>

While my the [previous work](https://mochiliu.github.io/portfolio/worm-behavior/) also utilize custom LabVIEW Virtual Instruments (VIs) for stimulation and capturing video, the control software here features a great deal more sophistication. It is able to track worms, extract centerlines along with other behavioral metrics, and draw spatially and temporally customized stimuli, all in real time at up to 30 Hz (the camera frame rate).

GUI of the real-time LabVIEW sotware.


Although it is possible to run the software on older machines, the software takes advantage of the high number of cores available to a modern computer to minimize the real-time behavioral feedback lag. The computers we picked employ the latest processor with 32 cores (Threadripper 3970X, AMD). These cores ensure that the LabVIEW environment has plenty of parallelized computing power to reduce the number of frames dropped in tracking, saving, and stimulus drawing. The computer also has 6 TB total of PCIe Gen4 solid state storage (SB-ROCKET-NVMe4-2TB, Sabrent) to enable us to save raw camera and projector images in real time. This way, we can reduce latency by doing the image compression in post-processing.

<figure>
  <center><img src="/assets/images/realtime/whole_field_of_view.gif" style="width:100%"></center>
  <figcaption>Downsampled video generated in post analysis of an experiment. It shows an overlay of the camera image and the stimulus image along with track animal paths. Note the actual experiment is running at much higher spatial and temporal resolution, 2048x1504 pixels and 30 Hz respectively.</figcaption>
</figure>

Using this instrument, we can not only study how the animal integrates head and tail touch stimulus, but also how it processes information during rare behaviors such as turning.

<figure>
  <center><img src="/assets/images/realtime/spatial_patterning.gif" style="width:100%"></center>
  <figcaption>An individual worm video plotted during post analysis for an experiment with independent head and tail stimulation. It shows the raw worm image, its centerline, and the overlaid stimulus image.</figcaption>
</figure>

<figure>
  <center><img src="/assets/images/realtime/turning_stim.gif" style="width:100%"></center>
  <figcaption>An individual worm video plotted during post analysis for an experiment with stimulation during turning. It shows the raw worm image, its centerline, and the overlaid stimulus image.</figcaption>
</figure>