# Architecture

## Overview

![](<.gitbook/assets/JJazzLabArchitecture (1).png>)

## Main NetBeans modules

### Core modules

[ChordLeadSheet](https://github.com/jjazzboss/JJazzLab/tree/master/core/ChordLeadSheet) Model classes for the chord leadsheet

[Harmony](https://github.com/jjazzboss/JJazzLab/tree/master/core/Harmony) Classes such as [Note](https://github.com/jjazzboss/JJazzLab/blob/master/core/Harmony/src/main/java/org/jjazz/harmony/api/Note.java), [Degree](https://github.com/jjazzboss/JJazzLab/blob/master/core/Harmony/src/main/java/org/jjazz/harmony/api/Degree.java), [TimeSignature](https://github.com/jjazzboss/JJazzLab/blob/master/core/Harmony/src/main/java/org/jjazz/harmony/api/TimeSignature.java), [ChordSymbol](https://github.com/jjazzboss/JJazzLab/blob/master/core/Harmony/src/main/java/org/jjazz/harmony/api/ChordSymbol.java), [ChordType](https://github.com/jjazzboss/JJazzLab/blob/master/core/Harmony/src/main/java/org/jjazz/harmony/api/ChordType.java), [Scale](https://github.com/jjazzboss/JJazzLab/blob/master/core/Harmony/src/main/java/org/jjazz/harmony/api/Scale.java), etc.

[Midi](https://github.com/jjazzboss/JJazzLab/tree/master/core/Midi) Various utility classes, including classes to manage drum maps

[MidiMix](https://github.com/jjazzboss/JJazzLab/tree/master/core/MidiMix) Model classes for the MidiMix (saved as .mix file). Controls the configuration of the output synth (bank select/program change, volume, panoramic, etc.) for a given song

[MusicControl](https://github.com/jjazzboss/JJazzLab/tree/master/core/MusicControl) Classes to control music playback (start, pause, etc.)

[Rhythm](https://github.com/jjazzboss/JJazzLab/tree/master/core/Rhythm) Model classes for a rhythm (style).

[RhythmDatabase](https://github.com/jjazzboss/JJazzLab/tree/master/core/RhythmDatabase) Manage the available rhythm instances on the system

[RhythmMusicGeneration](https://github.com/jjazzboss/JJazzLab/tree/master/core/RhythmMusicGeneration) Utility classes for music generation

[Song](https://github.com/jjazzboss/JJazzLab/tree/master/core/Song) Model classes for a song (saved as .sng file). A song mainly contains a ChordLeadSheet, a SongStructure and optional user phrases.

[SongStructure](https://github.com/jjazzboss/JJazzLab/tree/master/core/SongStructure) Model classes for a SongStructure

### App modules

[ActiveSong](https://github.com/jjazzboss/JJazzLab/tree/master/app/ActiveSong) Manages the active song (JJazzLab can open several songs in parallel, but only the active one is allowed to send Midi data).

[CL\_Editor](https://github.com/jjazzboss/JJazzLab/tree/master/app/CL_Editor) The graphical editor for a [ChordLeadSheet](https://github.com/jjazzboss/JJazzLab/blob/master/core/ChordLeadSheet/src/main/java/org/jjazz/chordleadsheet/api/ChordLeadSheet.java) object.

[MixConsole](https://github.com/jjazzboss/JJazzLab/tree/master/app/MixConsole) The graphical editor for a [MidiMix](https://github.com/jjazzboss/JJazzLab/blob/master/core/MidiMix/src/main/java/org/jjazz/midimix/api/MidiMix.java) object.

[SongEditorManager](https://github.com/jjazzboss/JJazzLab/tree/master/app/SongEditorManager) The central place where all song editors are managed

[SptEditor](https://github.com/jjazzboss/JJazzLab/tree/master/app/SptEditor) The graphical editor for a [SongPart](https://github.com/jjazzboss/JJazzLab/blob/master/core/SongStructure/src/main/java/org/jjazz/songstructure/api/SongPart.java).

[SS\_Editor](https://github.com/jjazzboss/JJazzLab/tree/master/app/SS_Editor) The graphical editor for a [SongStructure](https://github.com/jjazzboss/JJazzLab/blob/master/core/SongStructure/src/main/java/org/jjazz/songstructure/api/SongStructure.java) object.

### Plugins modules

[FluidSynthEmbeddedSynth ](https://github.com/jjazzboss/JJazzLab/tree/master/plugins/FluidSynthEmbeddedSynth)Manages the internal FluidSynth-based synth.

[JJSwing ](https://github.com/jjazzboss/JJazzLab/tree/master/plugins/JJSwing)A rhythm music generation engine for practicing jazz standards with a natural feel

[YamJJazz ](https://github.com/jjazzboss/JJazzLab/tree/master/plugins/YamJJazz)A rhythm music generation engine with 2 Yamaha style-based rhythm providers

