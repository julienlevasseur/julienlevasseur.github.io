---
layout: post
title: DIY Hybrid Mixer
description: 
tags:
  - Ardour
  - NetJack
  - Mixing
image:
---

The mixer market is a wide one filled with a large choice of different kind of analog and digital mixer.
As an hobbyist, especially with a limited budget, it can be tricky to find the perfect mixer, even more with specific needs that are different from the usual industry needs.

In my case, I was looking for:

- An at least 8 inputs mixer (3 stereo + 2 mono) with phantom alimentation capabilities.
- ADC/DAC + support of plugin (LV2, Ladspa, VST ...) with a (color) screen to manage plugins.
- At least one analog input (to plug a vynil without any signal conversion).
- A compact mixer (one that can stand on a desk and be carried for events).
- The possibility for extensions (adding more inputs/outputs)

I have used for a year a [Beringer Xenyx UFX1204](https://fr.audiofanzine.com/console-fw-usb-mlan/behringer/xenyx-ufx1204) (which as a compressor and inserts on its 4 mic/line (mono) inputs), but when I needed a noise gate, I had to consider buying an additional equipment (plus cables), and plug it as an insert. But what if the needs of a noise gate is on one of the stereo inputs ? Well, you can't.
The usage of the effects is also very limited. With one effect processor, you can only assign one effect at a time to the effect bus.

Considering these points, I've been looking for a replacement. I've considered:

- [Yamaha DM3](https://ca.yamaha.com/en/products/proaudio/mixers/dm3/index.html)
But it does not offer any 100% analog input (all input) and the price is not for every purse (even if it's not that high (~2000$ USD) for the quality of this compact mixer).

- [Yamaha 01v96i](https://ca.yamaha.com/fr/products/proaudio/mixers/01v96i/index.html)
But, like the DM3, you can't bypass the ADC on one or more inputs, plus, it's really pricey considering the older technology of the mixer, and it also feature a monochromic LCD display.

- [Presonus StudioLive 16.4.2](https://www.soundonsound.com/reviews/presonus-studiolive-1642)
But, similarly to the 01v96i, the technology of this mixer start to be outdated, the screen is a non graphic LCD display and the ergonomy of the mixer is terrible. It also didn't offer analogs inputs. Plus, after some research, I discovered that they have a tendency of breaking at anytime and there's no fix possible (you just have to throw it to the bin !).

Following these findings, I looked closer to the hybrid mixers, but it turns out that hybrid mixers are more like analog mixer with a USB DAC but analog workflow (like the Beringer Xenyx UFX1204), when I was looking for a double feature mixer with some digital strips and some analog strips.
It is at this point that I decided to build my own mixer.

## Considerations for building a DIY hybrid mixer

Based on the above research, I came to the conclusion that building my own custom mixer would be the best solution to address my custom needs.
Building my own mixer is not a new idea for me, already in the early 2000's I analysed the feasability of such an endeavour. By that time, for a hobbyist, the only achievable design was a analog mixer and to build one that would be of quality, the price of component and parts in addition to the complexity of the build made it more interesting to buy a used analog mixer (at that time, the early digital mixers (like the 01V) was out of reach for hobyists).

But 2 decades have passed and the technology evolved a lot. Lets looks at what is basically a mixer now:

* An interface (ususally composed of a surface control and one or multiple screens)
* A [DSP](https://en.wikipedia.org/wiki/Digital_signal_processor) (which is basically the combination of an ADC/DAC + a DSP + effects / dynamics processors)
* Inputs / Ouputs (physical connections, usually XLR / Jack plugs and of course, modern mixers have network connectivity (such as Dante, AVB, NetJack ...))

![digital_mixer_architecture](https://github.com/julienlevasseur/julienlevasseur.github.io/blob/master/assets/images/posts/2024_04_01-DIY-hybrid-mixer/digital_mixer_architecture.png?raw=true)

As of the date I write this, a lot of modern mixers are even physically built around these 3 elements architecture.
Here's an example from Waves where you can clearly distinguish the DSP + IOs (botoom chassis that is basically a DSP with IO extensions) and the Interface (controle surface + screen):

![Waves eMotino LV1](https://github.com/julienlevasseur/julienlevasseur.github.io/blob/master/assets/images/posts/2024_04_01-DIY-hybrid-mixer/waves_emotion_lv1.avif)

If you need to sonorise a band with almost no latency, you better look directly at one of those commercial mixers equiped with a proper DSP (designed to treat audio signal). But if like me, you are a hobbyist, you don't plan to use you rig for live band (I do some 1 or 2 mics gigs, more like conference or disco-mobile setup where few ms latency is totally acceptable), then we can talk.

The idea is to delegate the DSP role to two devices:

1. An USB Audio Recording Interface
2. A Computer

![diy_mixer_architecture](https://github.com/julienlevasseur/julienlevasseur.github.io/blob/master/assets/images/posts/2024_04_01-DIY-hybrid-mixer/diy_mixer_architecture.png?raw=true)

The USB Interface will handle:

- Physical audio inputs / outputs (analog and eventually (depending on the model) some digital ones too)
- ADC / DAC
- Effects and Dynamics (depending on the model and the software support)

> **Note**
>
> You can decide to use an "interfaceless" mixer (like the Behringer X-Air or Soundcraft Ui series) in place of an USB Recording Interface.
> I actually considered this option for awhile especially since their offer a really nice packaging and latency. But the price (even used) of these is really high and the interroperrability with human interfaces (screens and controle surfaces) is pretty limited (when not directly tied to the brand or stays in the virtual world with tablets UIs).

The Computer will handle:

- IO Routing, Visual interface and sound management (level, panning, etc) (via the DAW of your choice)
- Effects and Dynamics (in addition of the one offered by the usb interface or only the one from the DAW)
- Support of plugins (VST, LV2, Ladspa ...)
- Control Surface integration and customization (via the DAW)
- Network audio extension (Dante, AVB, NetJack ...)
- Integrated recording capabilities and even using your rig as a workstation for composition, mixing, producing ...

## Case Study - My Rig

Now that we've set the design, let's tie it to the  requirements:

1. An at least 8 inputs mixer (3 stereo + 2 mono) with phantom alimentation capabilities --> **USB Interface**
2. 
   1. ADC/DAC --> **USB Interface**
   2. support of plugin (LV2, Ladspa, VST ...) --> **Computer**
   3. with a (color) screen to manage plugins --> **Computer**
3. At least one analog input (to plug a vynil without any signal conversion) --> **Analog tiny Mixer**
4. A compact mixer (one that can stand on a desk and be carried for events) --> **Design** 
5. The possibility for extensions (adding more inputs/outputs) --> **USB Interface** + **Computer**

So, the idea is to build an audio workflow like this:

![diy_mixer_audio_workflow_diagram](https://github.com/julienlevasseur/julienlevasseur.github.io/blob/master/assets/images/posts/2024_04_01-DIY-hybrid-mixer/diy_mixer_audio_workflow_diagram.png?raw=true)

After some research, I choose the following devices for the USB interface and Control Surface:

- [Presonus AudioBox 1818VSL](https://legacy.presonus.com/products/AudioBox-1818VSL) - for its cost (~150$ USD on the used market and its features (XLR/Jack combo inpus, ADAT extensibility, GNU/Linux support out of the box). I also considered the [Tascam US-16x08](https://tascam.com/us/product/us-16x08/top), but is XLR only inputs and price made me pick the Presonus instead.)
- [Presonus FaderPort 8](https://www.presonus.com/fr/controllers/control-surface/faderport-series/2777100202.html) - for its full (and out of the box) support by [Ardour](https://ardour.org/).

> Both bought used

And since I needed a compact mixer with analog inputs, I picked a [Mackie Mix5](https://mackie.com/en/products/mixers/mix-series/mix5.html) as the analog device of the rig.
The choice was simple: I already had one and it totally fulfill the need:

- 2 Stereo inputs (one for the vynil table, one for the main output of the Presonus AudioBox)
- It is super compact
- Has also 1 Mic/Line mono input (useful for a additional / DJ microphone)

Which gives the following setup (the screen is not pictured here):
![diy_mixer_audio_workflow_visual](https://github.com/julienlevasseur/julienlevasseur.github.io/blob/master/assets/images/posts/2024_04_01-DIY-hybrid-mixer/diy_mixer_audio_workflow_visual.png?raw=true)

To keep it portable, I designed a square chassis of 20"1/2 side (the USB interface being outside of this chassis mainly for height considerations (I needed my rig to fit under a shelf), but also it offer more flexibility for extension in the future (like an ADAT IO interface)).

![diy_mixer_cad_1](https://github.com/julienlevasseur/julienlevasseur.github.io/blob/master/assets/images/posts/2024_04_01-DIY-hybrid-mixer/diy_mixer_cad_1.jpeg?raw=true)
![diy_mixer_cad_2](https://github.com/julienlevasseur/julienlevasseur.github.io/blob/master/assets/images/posts/2024_04_01-DIY-hybrid-mixer/diy_mixer_cad_2.jpeg?raw=true)
![diy_mixer_cad_3](https://github.com/julienlevasseur/julienlevasseur.github.io/blob/master/assets/images/posts/2024_04_01-DIY-hybrid-mixer/diy_mixer_cad_3.jpeg?raw=true)
![diy_mixer_cad_4](https://github.com/julienlevasseur/julienlevasseur.github.io/blob/master/assets/images/posts/2024_04_01-DIY-hybrid-mixer/diy_mixer_cad_4.jpeg?raw=true)
![diy_mixer_cad_5](https://github.com/julienlevasseur/julienlevasseur.github.io/blob/master/assets/images/posts/2024_04_01-DIY-hybrid-mixer/diy_mixer_cad_5.jpeg?raw=true)

Assembled:

![diy_mixer_pic_1](https://github.com/julienlevasseur/julienlevasseur.github.io/blob/master/assets/images/posts/2024_04_01-DIY-hybrid-mixer/diy_mixer_pic_1.jpeg?raw=true)
![diy_mixer_pic_2](https://github.com/julienlevasseur/julienlevasseur.github.io/blob/master/assets/images/posts/2024_04_01-DIY-hybrid-mixer/diy_mixer_pic_2.jpeg?raw=true)
![diy_mixer_pic_3](https://github.com/julienlevasseur/julienlevasseur.github.io/blob/master/assets/images/posts/2024_04_01-DIY-hybrid-mixer/diy_mixer_pic_3.jpeg?raw=true)
![diy_mixer_pic_4](https://github.com/julienlevasseur/julienlevasseur.github.io/blob/master/assets/images/posts/2024_04_01-DIY-hybrid-mixer/diy_mixer_pic_4.jpeg?raw=true)
![diy_mixer_pic_5](https://github.com/julienlevasseur/julienlevasseur.github.io/blob/master/assets/images/posts/2024_04_01-DIY-hybrid-mixer/diy_mixer_pic_5.jpeg?raw=true)
![diy_mixer_pic_6](https://github.com/julienlevasseur/julienlevasseur.github.io/blob/master/assets/images/posts/2024_04_01-DIY-hybrid-mixer/diy_mixer_pic_6.jpeg?raw=true)

### About this setup

The central piece of this mixer is [Ardour](https://ardour.org/). It maps the 8 inputs and 8 ouputs of the Presonus AudioBox to strips and add Buses, VCA (and even MIDI tracks) that are physically controlled by the Presonus FaderPort.
Ardour offers the support for plugins to manage effects ans dynamics. It is also the main visual interface of the mixer (in addition to the physical buttons of the FaderPort and its scrible screens).

The main output of the Presonus AudioBox is connected to the input 2/3 of the Mackie Mix5 and my vynil turntable is connected to the Mix5 input 4/5. the Mix5's main output is then connected to the speakers and is the main output of the hybrid mixer. The Mix5 Phones output can be used to monitor the global mix when I use a bus as phone bus in Ardour (each strip can have a send to this phone bus) and the Presonus AubioBox output 7 and 8 are used as stere output for this phones bus (There's a phone bus integration in the Audiobox, but since I delegate all DSP features to Ardour, I actually don't use it). Both phones out from the Mix5 and the AduiBox are connected to the TRS jack plugs from the rear of the mixer, these plugs are linked to the one in the front of the mixer (under the trackball support), which provide easy access to front phones plugs.

#### About latency

The `jack_iodelay` command allows to measure the roundtrip latency of the setup.

```bash
$ jack_iodelay
new capture latency: [0, 0]
new playback latency: [0, 0]
Signal below threshold...
new playback latency: [256, 256]
   612.240 frames     12.755 ms total roundtrip latency
	extra loopback latency: 228 frames
	use 114 for the backend arguments -I and -O ?? Inv
^C
$
```

My setup generate 12.755ms of latency, which is totally acceptable for the uasge of the mixer.
To check furthore on how to measure latency, I recommend you to check this [article](https://interfacinglinux.com/2024/01/08/measuring-round-trip-latency-on-linux/) from [interfacinglinux.com](interfacinglinux.com).

## Extensibility

One of the reason for choosing the Presonus AudioBox 1818VSL was its extensibility:

- 4 to 8 IO channels via ADAT (8 channels at 44.1kHz and 48kHz, 4 channels at 88.2kHz and 96kHz)
- 2 channels via S/PDIF

And the computer allows network extensibility via different protocols (proprietary or open).

### ADAT

The Presonus AudioBox 1818VSL accept up to 8 inputs and 8 outputs via ADAT. This is possible by connecting another device with ADAT capability (IOs).

Here's a list of some devices that can extend the AudioBox to make it a 16 IO device:

- Presonus AudioBox 1818VSL (of course)
- Presonus Digimax D8
- Behringer Ultragain ADA 8200
- Focusrite Scarlett OctoPre Dynamic
- ...

### Network

There is some well known (and used, especially in the professional industry) network protocols for audio, like: Dante.
The problem for a hobbyist with such protocol is that it is proprietary and requires expensive hardware.

There's some open solutions (often free) that allows to transport audio via network packets.
[NetJack](https://netjack.sourceforge.net/) is an interesting solution (anf an even more interesting one for me who decided to use Jack as the sound server API for the DAW that manage my mixer).

On a NetJack network, the server will the host with the IO interface (in my case: the mixer) and the client will be the host which adds IOs.
you could imagine a scenario where a NetJack client is used as an IO extension that offers the inputs/outputs from an USB interface connected to it to the NetJack server. (such a solution might introduce latency !)

The example pictured by the screenshot below, is another use case where NetJack is used as a digital connection to route the output of the [Mixxx](https://mixxx.org/) software (DJ software) to the mixer. (basically replacing 4 jack cables by one ethernet cable)

The client connects the outputs of Mixxx to the NetJack virtual plugs:
![netjack client graph](https://github.com/julienlevasseur/julienlevasseur.github.io/blob/master/assets/images/posts/2024_04_01-DIY-hybrid-mixer/diagram_3.png?raw=true)

The mixer sees the 4 `playback_*` inputs from Mixxx under:
![netjack server graph](https://github.com/julienlevasseur/julienlevasseur.github.io/blob/master/assets/images/posts/2024_04_01-DIY-hybrid-mixer/diagram_2.png?raw=true)

> **NOTE**
>
> NetJack only work via ethernet, you can't use WiFi to connect a client and a server via NetJack.
