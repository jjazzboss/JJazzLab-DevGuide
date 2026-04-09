---
description: An open application for developers
---

# Developping with JJazzLab

{% hint style="info" %}
JJazzLab is written in Java. Why ?

This choice reflects the project’s origins (started over 10 years ago, when Java was a common option for cross-platform desktop apps) and the nature of the problem: unlike a low-latency audio DAW, JJazzLab does not require hard real-time deadline management in the music generation engine. As a side note, modern Java is quite different from early versions and can deliver excellent performance.
{% endhint %}

## Updating the application

#### Apache NetBeans Platform

JJazzLab is based on the [Apache Netbeans Platform](https://netbeans.apache.org/tutorial/main/kb/docs/platform/) which provides a reliable and extensible architecture for cross-platform desktop applications.&#x20;

The NetBeans Platform manages the application lifecycle, window system, extension points, options, actions, and more.&#x20;

It uses NetBeans modules (different from java 9 JPMS modules) which group related Java classes into pluggable components, while managing visibility and dependencies between them.

#### Adding new features

Suppose you have a reharmonization algorithm that proposes alternate chord progressions (e.g., replacing `| A7 | Dmaj7 |` with `| Em7 A7 | Dmaj7 |`).

You can easily implement an action that, when user selects chords, suggests these alternatives in the existing chord popup menu.&#x20;

Then your code can be bundled in a module which is deployed like a plugin.&#x20;

#### Develop your own rhythm generation engine without hassle

Out of the box, JJazzLab provides the infrastructure (“plumbing”) that developers would otherwise need to build themselves: data management, user interface, MIDI management, playback control, music-generation control, etc.

In JJazzLab, the rhythm generation engine just receives a context (chords, song structure, tempo, etc.) and returns musical phrases (one per instrument) that form the backing track, ready for playback.&#x20;

The engine doesn’t have to manage real-time deadline: JJazzLab handles scheduling and synchronization.

## JJazzLab Toolkit

The [JJazzLab Toolkit](https://github.com/jjazzboss/JJazzLabToolkit) is a standalone jar file which contains the JJazzLab core features, the JJazzLab plugins and the required dependencies.&#x20;

Use the toolkit to use JJazzLab features independently of the JJazzLab application.
