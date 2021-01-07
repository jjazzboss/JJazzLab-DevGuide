# Main modules

JJazzLab-X relies on the [Netbeans Platform](https://netbeans.org/features/platform/features.html) which provides a modular infrastructure. You'll find below the most important modules of JJazzLab-X.

**ActiveSong** Manage the active song. JJazzLab-X can open several songs in parallel, but only the active one is allowed to send Midi data.

**ChordLeadSheet** The model for the chord leadsheet: it defines a number of bars and a list of ChordLeadSheetItems : CLI\_ChordSymbol \(chord symbol at a given position\) and CLI\_Section \(section name and time signatue at a given bar\).

**CL\_Editor** The graphical editor for a ChordLeadSheet object.

**Harmony** All the harmony related base classes, Note, Degree, TimeSignature, ChordSymbol, ChordType, Scale, etc.

**Midi** General Midi related classes.

**MidiMix** The model to store a set of instruments and settings \(e.g. which Midi bank/program change for the piano voice, which volume, reverb, transposition, etc.\). Also contains the MidiMixManager which associates a MidiMix to a song object.

**MixConsole** The graphical editor for a MidiMix object.

**MusicControl** Control the music playback \(start, pause, etc.\).

**Rhythm** The model for a rhythm \(=a style\). Also define the RhythmProvider interface which should be implemented by third-party rhythm generation engines.

**RhythmDatabase** Manage the available rhythms on the system. Upon startup the RhythmDatabase scans the available RhythmProvider implementations which provide the rhythm instances.

**RhythmMusicGeneration** Utility classes for music generation. Defines the MusicGenerator service provider interface.

**Song** The model for a song. A song mainly contains a chord leadsheet and a song structure.

**SongEditorManager** The central place where all song editors are created \(from scratch, loaded from file, etc.\) and managed.

**SongStructure** The model for a song structure: a list of SongParts. A SongPart defines a parent CLI\_Section \(see ChordLeadsheet model\), a rhythm, and the value of each of the rhythm parameters.

**SptEditor** The graphical editor for a SongPart.

**SS\_Editor** The graphical editor for a SongStructure object.

