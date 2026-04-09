# Build JJazzLab 5 from source code

Requirements:

* **jdk-23** or higher
* **maven 3** or higher
* **git** if you want to clone the repository (it can be downloaded as a ZIP file from github too).

## Preparation

These steps are required for building JJazzlab, both from the command line or from Netbeans:

* Clone the repository from [https://github.com/jjazzboss/JJazzLab](https://github.com/jjazzboss/JJazzLab) (or download and extract the source files)
* There is a file too big for git, you have to download it from [https://archive.org/download/jjazz-lab-sound-font/JJazzLab-SoundFont.sf2](https://archive.org/download/jjazz-lab-sound-font/JJazzLab-SoundFont.sf2) and move it to `plugins/FluidSynthEmbeddedSynth/src/main/soundfont`

### In Linux & MacOS, install _fluidsynth_ package

On both Linux and MacOS, you also need the native `fluidsynth` package. Check if it is installed with `fluidsynth --version` . If it's not yet there, install it with:

* `sudo apt-get install fluidsynth`  on Debian based distributions,
* `brew install fluidsynth` for Homebrew users on MacOS,
* for other Linux distributions and other MacOS installation options see [https://github.com/FluidSynth/fluidsynth/wiki/Download](https://github.com/FluidSynth/fluidsynth/wiki/Download)

## Build from the command line

With the preparation done, you only need to execute the build and run the application:

* In the project root, run `mvn clean install`
* Go to app/application and run `mvn nbm:cluster-app nbm:run-platform`&#x20;

The application should open up, the output of the application will be visible in the terminal that launched the app.

## Build from Netbeas IDE

Because JJazzlab uses Apache Netbeans Platform, Netbeans IDE is preferred for development. Follow these steps to build and run the application:

* Go to `File` menu, `Open project`  and point to the folder with JJazzlab source code
* You will see a project named `JJazzLab parent [master]` , right click it and select `build`&#x20;
* Expand `JJazzLab parent / modules`  using the `>` symbol on the left, and locate the module called `JJazzLab App`&#x20;
* Open that module by either double clicking on it or using right click and `Open Project`&#x20;
* Scroll up on the projects panel and locate `JJazzLab App [master]`&#x20;
* Right click the project name and use `Run`&#x20;

The application should open up, the output of the application will be visible in the output panel in the IDE.

## Troubleshooting

If the instructions don't work as expected, consider creating an issue in [github JJazzLab repository](https://github.com/jjazzboss/JJazzLab/issues) or asking in [jjazzlab forums](https://jjazzlab.freeforums.net/).
