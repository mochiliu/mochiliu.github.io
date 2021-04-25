---
title: "Project Synesthesia"
excerpt: "Can a machine translate one senory phenomenon into another?"
header:
  image: /assets/images/unsplash-gallery-image-1.jpg
  teaser: assets/images/unsplash-gallery-image-1-th.jpg

---
Keywords: Deep Learning, Auto Encoder, Tensorflow, Python, Game of Life, Cloud Compute, Music Synthesis, IoT, MQTT, Home Assistant


Can a machine translate one senory phenomenon into another? Or are experiences solely the domain of biological beings?

These questions motivated a series of inter-connected pet projects I worked on in the past few years. 

Before we dive deeper, a warning to the outside observer: the path here taken is purely exploratory as opposed to focused on some end product. The code written are cobbled together based on the momentary whims of inspiration and are not meant for presentation to anyone else. It originally started as a HackPrinceton project that I worked on with Genna Gliner, Ugne Klibaite, and Max Homilius, but it has since then evolved into a solo venture.

### Light Board Hardware

The hardware of this project features a custom-made light board with 900 [individually addressable LEDs](https://github.com/richardghirst/rpi_ws281x) arranged 30x30, a Raspberry Pi 3B+, a sound system, and a 300W 5V DC power supply. When complex machine learning models are needed, the control data is streamed over wifi in real-time via the MQTT protocol from a laptop or desktop with a little more computational oomph than a raspberry pi.


<figure>
  <center><img src="/assets/images/projectsyn/lightboard.gif" style="width:50%"></center>
  <figcaption>The light board is composed of 900 individually addressible LEDs that can be updated at 11Hz.</figcaption>
</figure>

### Game of Life

The first software project is a [python implementation](https://github.com/benosteen/conways-game-of-life) of a modified version of [`Conway's game of life`](https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life) known as [High life](https://en.wikipedia.org/wiki/Highlife_(cellular_automaton)), designed specifically for the LED board. It has 30x30 cells that have continuous boundary conditions, meaning the cells wrap around the four sides like an unfurled sphere (i.e the four corners of the board are actually the same point). The original game of life features binary "alive" or "dead" cell, but to make it more interesting I added some color to the game as well. The initial cells in the very first iteration are given a randomly chosen color when it is "spawned", and the subsequent iteration of cells that come alive have their color attributes inherited from the "parent" cells along with a slight random genetic mutation. I picked the game of life as a foundation of my exploration because it  can consistently produce qualitatively "interesting" visual patterns from a random seed. 


<figure>
  <center><img src="/assets/images/projectsyn/gameoflife.gif" style="width:50%"></center>
  <figcaption>A demo of the game of life with periodic boundary conditions and a genetic coloring scheme.</figcaption>
</figure>

It is the ultimate example of emergence - simple predetermined rules in the micro-scale produces complex phenomena in the macro-scale. Starting from a random seeded board, and evolving the game state forward, we can observe a multitude of patterns in space and time that appear simultaneously random yet coherent. Although no two randomly instantiated game of life board can provide extremely divergent outcomes, familiar patterns arise if you are a careful observer.

As a human watching a game of life plays out, I can't help but try to narrate it, translating a visual experience into another form. However, I begin to ponder if this would be possible for an algorithm to do as well, and to what extent can this transformation preserve the essence of the orginial. So I set out to find algorithms that can translate a visual experience of a game of life evolving through time into an auditory one.

But how can a game of life to produce music? The following sections produce some solutions, but the exploration is still ongoing. 

###The Mechanical Piano

The simplest solution that I came up with is to use the alive cells as notes, as if a mechanical piano was played with perforated paper. The board is read from one side to the other in columns. With some fixed interval, the notes are played out loud using [fluidsynth](https://www.fluidsynth.org/). To ensure the notes do not sound too discordant, we will enforce a major pentatonic scale. My personal favorite instruments are the vibraphone and seashore.

Vibraphone and Seashore clips

###Deep Learning

This rule based translation of a visual board into muscial notes is simple, but it feels rigid. The same game of life board always produce the exact same sounds. Just like there are hundreds of human languages to describe the same experience (not to mention a vast number of ways to narrate even in the same language), I wanted a method that is able to recreate dynamics of the game of life in sound space using different mappings. But to do so, I needed ways to capture the "essence" of all possible game of life boards and some predefined sound space, because they can be astronmically big. Even a small 30x30 game of life board has 2^900 possible states, much larger than the number of atoms in the universe (2^256). However, much of this state space is very unlikely to be occupied, as the rules of the game of life quickly produces familiar patterns. This can also be said of musical melodies, with a space that is even larger than that of the game of life.

This desire to find represenations that reduce the dimensionality led me to a class of neural networks known as [autoencoders](https://en.wikipedia.org/wiki/Autoencoder). Through the magic of deep learning, we can perform this dimensionality reduction by training of thousands of game of life configurations that are naturally evolved from random seeds. The autoencoder simultaneously trains both an encoder and a decoder. The encoder takes the 900 dimensional game of life image and outputs a much lower dimensional latent vector. The decoder takes the low dimensional latent vector and tries to recreate the original game of life board. The training process seeks to minimize the difference between the original board and the regenerated board that has low dimensional information bottleneck. I found for this instance, the smallest latent vector that is a power of 2 is 32 dimensions. 

PICS OF BOARD AND REGENERATED

Note the autoencoder is trained to generate binary black and white images. It does not have access to color information. The autoencoder code is forked from [BAVE-tf](https://github.com/alecGraves/BVAE-tf), with custom convolutional and dense layers designed for the game of life by me. See [BAVE-tf](https://github.com/mochiliu/BVAE-tf) for source code. I used my introductory $300 of Google Cloud Compute credits to train some of these models. After burning through my credits, I invested in a entry level Nvidia GPU for training. 

For autoencoders that can generate music, I used a pre-trained model from Google's [Project Magenta](https://magenta.tensorflow.org/music-vae). These pre-trained models are especially helpful because they are trained on high quality data with computational resources that I can only drool at. The researchers here trained autoencoders that can represent 2 bar melodies or drumlines.

Using these two autoencoders, I can then translate game of life boards into musical beats or melodies by leveraging an interesting property of autoencoders, namely, points close to each other in the lower dimensional latent space are also close to each other in the higher dimeensional space. Due to this characteristic, I can captures the relative movement in game of life space and translate that into a similar movement in the music space, giving an dynamic to the music generation that is "inspired" by the evolving game. I do this by connecting the trained encoder of the game of life board to the trained decoder for music generation at the latent space. Because the dimensionalities of the latent spaces for the music autoencoders are much larger, I pick a random 32 dimensional subspace for the much larger (size 128 or 256) space and a random starting point before starting the game. For each iteration of the game of life, I calculate how much deviation occurred in the game of life latent space, and translate an equal amount in the music latent space via the predefine subspace. Some sample results are shown below.

SAMPLE RESULTS

Although with much effort, I was able to get Tensorflow to run on a Raspberry Pi 3, it doe not offer nearly enough memory or computational speed to perform inference with these autoencoder models in real-time. As a result, I built a way to stream the game of life and music generation from my laptop to the Raspberry Pi using JSON through the [MQTT](https://en.wikipedia.org/wiki/MQTT)protocol.

Although interesting, the results only begs for further development. I am always looking for ideas on this front :)

