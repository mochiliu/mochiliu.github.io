---
title: "Real-time tracking and stimulus targeting"
excerpt: "I designed an instrument that can probe worm behavioral response to spatially coherent inhibitory and excitatory optogenetic stimuli, with real-time behavior segmentation and stimulus targeting for many animals in parallel."
header:
  image: /assets/images/realtime/whole_field_of_view.gif
  teaser: /assets/images/realtime/whole_field_of_view.gif
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

While highly informative to our understanding of worm touch circuitry, the my [previous work](https://mochiliu.github.io/portfolio/worm-behavior/) has several limitations that my current research efforts seek to overcome: (1) the light stimulus used is only a single color, which prevents multiplexing excitatory and inhibitory stimulation in the same experiment; (2) the stimulus intensity is shared for entire plate, which means we cannot study how the touch stimulus affects the head or the tail separately; and (3) stimulation was agnostic to the animal’s behavior, which makes it challenging to gather stimulus reponse during rare behaviors. So I designed an instrument that can probe worm behavioral response to spatially coherent inhibitory and excitatory optogenetic stimuli, with real-time behavior segmentation and stimulus targeting for many animals in parallel. This work is currently in preparation for publication.

<figure>
  <center><img src="/assets/images/realtime/Experimental_Design_API.png" style="width:50%"></center>
  <figcaption>Cartoon layout of the Real-Time Multimodal Optogenetic Assay System. A projector is used for targeted optogenetic stimulus delivery based on real-time tracking of worms on an agar plate. The green channel is reserved for spatial and temporal calibration, while the red and blue channels are used for optogenetics.</figcaption>
</figure>

<figure>
  <center><img src="/assets/images/realtime/instrument.jpg" style="width:50%"></center>
  <figcaption>Photo of the instrument. It features a projector on top and a camera on the bottom sandwiching a infrared LED ring that holds a plate of worms.</figcaption>
</figure>

<figure>
  <center><img src="/assets/images/realtime/instrument_top.jpg" style="width:50%"></center>
  <figcaption>Photo of the instrument from the top showing the DLP projector, control board, and a custom 3D printed mount.</figcaption>
</figure>

While my the [previous work](https://mochiliu.github.io/portfolio/worm-behavior/) also utilize custom LabVIEW Virtual Instruments (VIs) for stimulation and capturing video, the control software here features a great deal more sophistication. It is able to track worms, extract centerlines along with other behavioral metrics, and draw spatially and temporally customized stimuli, all in real time at up to 30 Hz (the camera frame rate).

<figure>
  <center><img src="/assets/images/realtime/GUI.gif" style="width:100%"></center>
  <figcaption>GUI of the real-time LabVIEW software. While only a single worm is displayed in detail with low refresh rate, all of the worms are being tracked simulataneouly at 30 Hz.</figcaption>
</figure>

<figure>
  <center><img src="/assets/images/realtime/live.gif" style="width:50%"></center>
  <figcaption>Live experiment as seen from the side. Individual worms are targeted for stimulation while a temporal calibration pattern is projected in green.</figcaption>
</figure>

Although it is possible to run the software on older machines, the software takes advantage of the high number of cores available to a modern computer to minimize the real-time behavioral feedback lag. The computers we picked employ the latest processor with 32 cores (Threadripper 3970X, AMD). These cores ensure that the LabVIEW environment has plenty of parallelized computing power to reduce the number of frames dropped in tracking, saving, and stimulus drawing. The computer also has 6 TB total of PCIe Gen4 solid state storage to enable us to save raw camera and projector images in real time. This way, we can reduce latency by doing the image compression in post-processing.

<figure>
  <center><img src="/assets/images/realtime/whole_field_of_view.gif" style="width:100%"></center>
  <figcaption>Downsampled video generated in post analysis of an experiment. It shows an overlay of the camera image and the stimulus image along with track animal paths. Note the actual experiment is running at much higher spatial and temporal resolution, 2048x1504 pixels and 30 Hz respectively. The green dots in the center encodes a timestamp in binary that allows us to compute the latency from when the camera takes an image until the stimulus is delivered to the tracked animals in that image. Typically, this "closed loop lag" is 4 frames or 133ms. </figcaption>
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

Finally, to even further increase throughput, we scaled from a single prototype intrument to 4 identical instruments.

<figure>
  <center>4 instruments running simulataneously, allowing us to assay hundreds of animals at once.<img src="/assets/images/realtime/four_rigs.jpg" style="width:100%"></center>
  <figcaption></figcaption>
</figure>
