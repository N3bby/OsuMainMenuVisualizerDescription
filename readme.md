# Osu music visualizer

## Introduction

This document contains a description of how I made the visualizer in [this video](https://www.youtube.com/watch?v=iVgbb_uGno4)  
It was requested by [Deemo Lee](https://www.youtube.com/channel/UCG_HV-Qbz3uS6ifu43j3ZLA) in the comment section.

I split it up into three parts that should react to the music.  
These sections will only cover some techniques, no explanation of how to code it.

## Bars

This section covers the audio bars on the side of the logo

### Audio spectrum

In Unity, you can use [this](https://docs.unity3d.com/ScriptReference/AudioSource.GetSpectrumData.html) function to get audio spectrum data.  
It fills a given float array with volume values for each sample (which is a range of frequencies) (I believe the min and max are 20-20.000hz)

Music usually contains more lower-frequency sounds, so you can cut off a large amount of the higher frequencies (otherwise you'll have a lot of bars that won't move at all)

Each audio bar is then mapped to a certain sample range (based on how many bars there are and what the final range is)

### Visualisation

The bars are just basic planes (or cubes) that are instantiated at runtime.  
I'd suggest making it so you can easily change the amount of them. (bit of math involved, but should not be too hard)

The bars are then scaled based (and moved) based on the current volume of their sample range.

Don't update the bars every frame though, otherwise your result will jump around a lot.  
The way I handled it in my project is by only updating the scale if the new scale would be higher than the current scale.  
I'm also slowly lerping the scale to 0. The transparency of the bar is also changed based on it's scale I think.

You can see that I still have issues with some bars being more active than others.  
The official osu! client kind of mitigates this problem by rotating the sample ranges around (+ there are also multiple bars stacked onto each other?)  
Here's an example (follow my cursor to see what I mean)
<img src="https://github.com/Razacx/OsuMainMenuVisualizerDescription/blob/master/img/rotationgif.gif" width="400">

## Logo

### Beat detection

I personally just checked the lower frequencies for sudden spikes in volume.  
There are better solutions to this though, a quick search on google will give you a plethora of simple and complex solutions.

### Visualisation

Whenever a beat occurs, I scale the logo to its minimum size and instantiate and 'audio wave' object.  
The wave object is just an image like this that scales outwards and increases in transparency
<img src="https://github.com/Razacx/OsuMainMenuVisualizerDescription/blob/master/img/audio_wave.png" width="250">

The logo also lerps back to its original size.  
A system that might be better is making the logo have some sort of 'spring' behaviour, where you can add a force to the spring whenever there is a beat.

## Kiai Time

This is something I didn't really include in my project, but the official client does this.  
Whenever the music enters kiai mode (which can be parsed from beatmap files), there are light flashes on the edge of the screen and particles are released.
