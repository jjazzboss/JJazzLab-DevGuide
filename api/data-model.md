# Data model

## `Song` 

The [Song](https://github.com/jjazzboss/JJazzLab-X/blob/master/Song/src/org/jjazz/song/api/Song.java) object is the main data model. It is mainly composed of a [ChordLeadSheet](https://github.com/jjazzboss/JJazzLab-X/blob/master/ChordLeadSheet/src/org/jjazz/leadsheet/chordleadsheet/api/ChordLeadSheet.java) and a [SongStructure](https://github.com/jjazzboss/JJazzLab-X/blob/master/SongStructure/src/org/jjazz/songstructure/api/SongStructure.java).

The `ChordLeadSheet` object is the model of the Chord leadsheet editor. It holds the sections and the chord symbols.

The `SongStructure` object is the model of the Song structure editor. It holds the [SongParts](https://github.com/jjazzboss/JJazzLab-X/blob/master/SongStructure/src/org/jjazz/songstructure/api/SongPart.java), each one being linked to a parent section, and each one defining a set of `RhythmParameter` values.

Combining the `SongStructure` and the `ChordLeadSheet` will give you the "unrolled" chord lead sheet for which the backing track must be generated.

## `MidiMix` 

The [MidiMix](https://github.com/jjazzboss/JJazzLab-X/blob/master/MidiMix/src/org/jjazz/midimix/MidiMix.java) stores the Midi configuration of a given `Song`. It is mainly used by the framework to control the Midi output device. Your `MusicGenerator` will use it only to retrieve the Midi channel associated to a `Rhythm` track \(`RhythmVoice`\).

## Helper classes

### Harmony module

Supporting classes used in the `Song`: [TimeSignature](https://github.com/jjazzboss/JJazzLab-X/blob/master/Harmony/src/org/jjazz/harmony/TimeSignature.java), [ChordSymbol](https://github.com/jjazzboss/JJazzLab-X/blob/master/Harmony/src/org/jjazz/harmony/ChordSymbol.java), [Degree](https://github.com/jjazzboss/JJazzLab-X/blob/master/Harmony/src/org/jjazz/harmony/Degree.java), etc.

### RhythmMusicGeneration module

Supporting classes useful for your `MusicGenerator`:

* [Phrase](https://github.com/jjazzboss/JJazzLab-X/blob/master/RhythmMusicGeneration/src/org/jjazz/rhythmmusicgeneration/Phrase.java) and [NoteEvent](https://github.com/jjazzboss/JJazzLab-X/blob/master/RhythmMusicGeneration/src/org/jjazz/rhythmmusicgeneration/NoteEvent.java): the `MusicGenerator` needs to create a `Phrase` for each rhythm track.
* [ContextChordSequence](https://github.com/jjazzboss/JJazzLab-X/blob/master/RhythmMusicGeneration/src/org/jjazz/rhythmmusicgeneration/ContextChordSequence.java): a helper class to get the "unrolled" chord leadsheet descrive above.
* [Grid](https://github.com/jjazzboss/JJazzLab-X/blob/master/RhythmMusicGeneration/src/org/jjazz/rhythmmusicgeneration/Grid.java): music phrase manipulation methods
* etc.

