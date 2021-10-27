# Build from source code

### Make sure you have Java OpenJDK 11 installed

If not, install the latest version from [adoptopenjdk.net](https://adoptopenjdk.net).

### Download and install Netbeans IDE 12.3 (or later) from [Apache Netbeans](https://netbeans.apache.org) website

This is the development environment and platform on which JJazzLab-X is built.

### Start Netbeans IDE

### Retrieve the JJazzLab-X code source project from GitHub

1. Netbeans IDE menu **Team/Git/Clone**
2. Enter repository address **https://github.com/jjazzboss/JJazzLab-X.git **
3. Leave user and password blank
4. Press **Next **and select the **master **branch (or another branch, if you know what you're doing)
5. Let Netbeans open the **JJazzLab-X** project from the cloned files

### Build the JJazzLab-X application

1. Select the **JJazzLab-X** project, right-click **Build**. \
   When it's done you should see "BUILD SUCCESSFUL" in the **Output **window.

### Run JJazzLab-X

1. Select the **JJazzLab-X** project, right-click **Run**.

JJazzLab-X should start with the default language set up for your computer, e.g. if you use Windows in the German language, you should see JJazzLab in German. If a source phrase is not translated in the target language, the English one is used.

{% hint style="danger" %}
* The application will have no branding and no rhythms, this is normal, those come with the JJazzLab distribution
* The main menu bar (File, Edit, Tools, Window, Help, Check for updates) and a few window popup-menus will remain in English, this is also normal
{% endhint %}

### Force JJazzLab-X to run with a different language

You can't switch the language from within the JJazzLab-X application when it is run from Netbeans IDE. But it's still possible to change language for test purposes:

1. In Netbeans, **Projects **tab, open the **JJazzLab-X** project, then the **Important Files** folder
2. Open **Project Properties**
3. Find "**run.args.extra=...**" and insert "**--locale xx:XX **" right after the "="\
   For example: "**run.args.extra=--locale fr:FR \\**"

Use fr:FR for French, de:DE for German, en:US for English, it:IT for Italian, es:ES for Spanish, ja:JP for Japanese, zh:CN for Chinese (mandarin).
