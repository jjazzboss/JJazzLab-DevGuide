# JJazzLab-X

JazzLab-X is an open-source platform dedicated to backing tracks generation -some people prefer to say “auto-accompaniment applications”.

The objective is to encourage third-party developers to add new features and new powerful music generation engines: out of the box, the JJazzLab-X platform provides all the infrastructure, all the “plumbing” that, before, every developer had to write themselves. Developers can save a huge amount of work by focusing only on their music generation engine.

![](.gitbook/assets/jjazzlab-x-architecture.jpg)

JJazzLab-X can host any number of music generation engines as plugins. When you press the Play button, the platform sends all the song data to the music generation engine of the song’s rhythm \(e.g. bossa nova\). The engine generates the Midi data for the backing tracks and sends it back to the platform, which can play it.



