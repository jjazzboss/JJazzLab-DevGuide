---
description: Creating your own music generation engine
---

# Music generation

## Overview

JJazzLab framework → RhythmProvider → Rhythm → **MusicGenerator** → phrases → post-process

{% hint style="success" %}
The engine doesn’t have to care about real-time deadlines: JJazzLab handles scheduling and synchronization.
{% endhint %}

## RhythmProvider

Your engine must implement the [`RhythmProvider`](https://github.com/jjazzboss/JJazzLab/blob/master/core/Rhythm/src/main/java/org/jjazz/rhythm/spi/RhythmProvider.java) interface with the `@ServiceProvider` annotation, so that the [`RhythmDatabase`](https://github.com/jjazzboss/JJazzLab/blob/master/core/RhythmDatabase/src/main/java/org/jjazz/rhythmdatabase/api/RhythmDatabase.java) implementation can automatically find it upon startup. The used Netbeans mechanism is described [here](https://netbeans.apache.org/wiki/main/netbeansdevelopperfaq/DevFaqLookupDefault/) (lookup).&#x20;

**Example**: See [RhythmStubProviderImpl.java](https://github.com/jjazzboss/JJazzLab/blob/master/core/RhythmStubs/src/main/java/org/jjazz/rhythmstubs/RhythmStubProviderImpl.java) for a simple `RhythmProvider` implementation example.

## Rhythm, RhythmVoices and RhythmParameters&#x20;

The engine provides  [`Rhythm`](https://github.com/jjazzboss/JJazzLab/blob/master/core/Rhythm/src/main/java/org/jjazz/rhythm/api/Rhythm.java)  instances to the framework.&#x20;

The `Rhythm` interface notably defines :&#x20;

* name, time signature, preferred tempo, feel, etc.
* [`RhythmVoices`](https://github.com/jjazzboss/JJazzLab/blob/master/core/Rhythm/src/main/java/org/jjazz/rhythm/api/RhythmVoice.java) : the supported tracks (**Drums**, **Bass**, **Piano**, etc.). Each `RhythmVoice` defines the recommended Midi instrument and settings for the track.
* [`RhythmParameters`](https://github.com/jjazzboss/JJazzLab/blob/master/core/Rhythm/src/main/java/org/jjazz/rhythm/api/RhythmParameter.java) : the "control knobs" given to the user to modulate music generation. Common rhythm parameters are **Variation**, **Intensity**, **Mute**, etc. You see them in the Song Structure Editor, their value can be set for each Song Part. The framework provides default UI widget for each rhythm parameter type, but you can define your own UI widget if required.

{% hint style="info" %}
`Rhythm` instances are actually provided via `RhythmInfo` instances, which are used by the [`RhythmDatabase`](https://github.com/jjazzboss/JJazzLab/blob/master/core/RhythmDatabase/src/main/java/org/jjazz/rhythmdatabase/api/RhythmDatabase.java) for its cache file. They represent what the user sees in the rhythm selection dialog.
{% endhint %}

## MusicGenerator

A `Rhythm` instance should implement the [`MusicGeneratorProvider`](https://github.com/jjazzboss/JJazzLab/blob/master/core/RhythmMusicGeneration/src/main/java/org/jjazz/rhythmmusicgeneration/spi/MusicGeneratorProvider.java) interface.&#x20;

Actual music generation is performed by the provided [`MusicGenerator`](https://github.com/jjazzboss/JJazzLab/blob/master/core/RhythmMusicGeneration/src/main/java/org/jjazz/rhythmmusicgeneration/spi/MusicGenerator.java) instance. It receives a song context (chord leadsheet, song structure, tempo, etc.) and returns musical phrases (one per instrument) that form the backing track.

**Example**: See [RhythmStub.java](https://github.com/jjazzboss/JJazzLab/blob/master/core/RhythmStubs/src/main/java/org/jjazz/rhythmstubs/api/RhythmStub.java) for a simple `Rhythm` implementation example.

**Example**: See [DummyGenerator.java](https://github.com/jjazzboss/JJazzLab/blob/master/core/RhythmMusicGeneration/src/main/java/org/jjazz/rhythmmusicgeneration/api/DummyGenerator.java) for a simple `MusicGenerator` implementation example.

## Post-processing

After generation, the framework applies engine-independent post-processing to the returned phrases :&#x20;

* muting instruments in a song part
* per-channel velocity shift
* volume fade-out
* tempo factor changes
* etc.

The details of post-processing tasks performed by the framework are provided in the `MusicGenerator` interface comments.
