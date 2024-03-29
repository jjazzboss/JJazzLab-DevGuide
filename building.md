# Getting started

{% hint style="warning" %}
You need to have previously built [JJazzLab-X from source code](build-from-source-code.md) using Netbeans IDE.
{% endhint %}

JJazzLab-X is based on the [Apache Netbeans Platform](https://netbeans.apache.org/kb/docs/platform.html) (formerly Netbeans RCP for Rich Client Platform). It provides a reliable and extensible architecture for desktop application which manages the application life cycle, the window system, extension points, options, actions, etc.

Each distinct feature in a Netbeans Platform application can be provided by a distinct Netbeans module, which is comparable to a plugin. A Netbeans module is a group of Java classes that provides an application with a specific feature. Most of the directories you see in the [JJazzLab-X root directory on GitHub](https://github.com/jjazzboss/JJazzLab-X) are the JJazzLab-X modules.

So unless you want to fix the JJazzLab-X code itself, you will probably start by creating your own Netbeans module.&#x20;

## Create your Netbeans module

{% hint style="info" %}
In this example we want to add a new feature which proposes re-harmonized chord progressions. The reharmonization action will operate on the chord symbols selected by the user, and should be accessible via the chord symbol popup menu.
{% endhint %}

Let's first create a new module to hold our code.

In the Netbeans IDE, select JJazzLab-X/modules then **Add New...** in the popup menu.

1. In the Netbeans IDE, select JJazzLab-X/modules then **Add New...** in the popup menu
2. Enter any name, e.g. "Reharmonize"
3. Specify any code name base (java package name), e.g. "org.myself.reharmonize"

You new module will appear in the JJazzLab-X modules list. Open it. You should have something like this:

![](<.gitbook/assets/2021-05-30 18\_38\_20-Window.png>)

The **Important Files** folder contain the module configuration files, which usually you don't have to modify manually (this is done automatically by the IDE, typically using the module Properties dialog).&#x20;

#### Adjust module properties

Select the Reharmonize module then **Properties **in the popup menu. The important settings are:

* **Source level**: must be set to 11 (enable Java 11 language features)
* **Libraries **: dependencies on other modules (JJazzLab-X modules or Netbeans application framework modules), plus possibly dependencies on external libraries used by your module
* **API versioning**: this is where you declare the public packages of your module, i.e. the packages visible by other modules when they add your module as a dependency

**Example **: below are the properties of the JJazzLab-X **Quantizer **module:

As you see below, the Quantizer module has dependencies on 2 JJazzLab-X modules: ChordLeadSheet and Harmony. The 2 other ones (Base Utilities API and Lookup API) are modules from the Netbeans application framework.

![](<.gitbook/assets/2021-05-30 20\_17\_59-Window.png>)

The Quantizer module has only a single API package, which is made public to other modules, via the Public Packages list in the API versioning category.

![](<.gitbook/assets/2021-05-30 20\_18\_34-Window.png>)

## Create an action

We want to create a "Reharmonize" action which should be callable from the chord symbol popup menu, and should operate on the user-selected chord symbols.&#x20;

The best way to start is to look for a similar action and copy the code. Actions classes are mainly found in the following modules:

* [CL\_Editor** **](https://github.com/jjazzboss/JJazzLab-X/tree/master/CL\_Editor/src/org/jjazz/ui/cl\_editor)module : Chord Leadsheet editor actions such as insert bar, transpose chord symbols, etc.
* [SS\_Editor ](https://github.com/jjazzboss/JJazzLab-X/tree/master/SS\_Editor/src/org/jjazz/ui/ss\_editor)module : Song Structure editor actions such as delete song part, change rhythm, etc.
* [MixConsole ](https://github.com/jjazzboss/JJazzLab-X/tree/master/MixConsole/src/org/jjazz/ui/mixconsole)module : save default rhythm mix, mute all, etc.
* [MusicControlActions ](https://github.com/jjazzboss/JJazzLab-X/tree/master/MusicControlActions/src/org/jjazz/ui/musiccontrolactions)module: play, stop, etc.&#x20;
* [SongEditorManager ](https://github.com/jjazzboss/JJazzLab-X/tree/master/SongEditorManager/src/org/jjazz/songeditormanager)module: new song, open song, duplicate song, etc.

The chord leadsheet editor action [TransposeDown** **](https://github.com/jjazzboss/JJazzLab-X/blob/master/CL\_Editor/src/org/jjazz/ui/cl\_editor/actions/TransposeDown.java)is enabled when user has selected one or more chord symbols, so this could be our code basis. Let's refactor-copy the java file in our module :

In the Netbeans IDE:

1. Open the **CL\_Editor** module, find, select and copy **TransposeDown.java**.&#x20;
2. Open the **Reharmonize **module, select the **org.myself.reharmonize** package, and choose **Paste **(refactor copy)
3. A dialog appears, set the new name to **Reharmonize.java** and press Refactor, the file is created.

**Reharmonize.java** has many errors: this is due to the missing module dependencies. You could manually update the module properties as explained above, but there is an easier way, directly from the editor, as shown below.

![](<.gitbook/assets/2021-05-30 21\_46\_05-Window.png>)

Roll the mouse over the light bulb on the left margin, and click on the popup **Seach Module Dependency for org.xxx**.  This brings the **Add Module Dependency** with the Filter field initialized with the dependency to be searched. Wait a few seconds and the relevant dependency should appear: select it and press OK.

![](<.gitbook/assets/2021-05-30 21\_51\_42-Window.png>)

Repeat the same operation until all dependencies are fixed.&#x20;

### Action annotations

Now you should have only one error in the file:

![](<.gitbook/assets/2021-05-30 21\_59\_36-Window.png>)

Netbeans uses annotations to facilitate action declaration:&#x20;

* In the **@ActionID** declaration, replace the **id **string by a string of your choice, e.g **org.myself.reharmonize.** This id can be used from any other module to run the action via the Netbeans API Actions.forID().

The action displayName is **#CTL\_TransposeDown**, and the leading # means the string value is searched in the localization **Bundle.properties** localization file in the same package (so it can be easily internationalized). As we copied only the .java file, the string definition is missing in our **Bundle.properties** file. Let's fix this:

1. Change all the **CTL\_TransposeDown** strings to **CTL\_Reharmonize** in **Reharmonize.java**
2. Edit **Bundle.properties** and add a line with **CTL\_Reharmonize=Reharmonize chord progression**

The **@ActionReference** puts a reference to this action in the Netbeans virtual file system (created at runtime), in the **Actions/ChordSymbol** directory. When user shows the chord symbol popup menu, JJazzLab-X takes all action references found in this directory and creates the related menu entries, using the **position **value to order them.

1. In **@ActionReference** change position value to **415**, so our action will appear in the popup menu after the TransposeDown action.

Now the module should be compilable. Select the **Reharmonize **module then **Build **from the popup menu.

Run JJazzLab-X, then in a song select a chord symbol and show the popup menu: our action should be there, as shown below. If you select it it will transpose down the chords symbols, as we have not yet changed the action code itself.

![](<.gitbook/assets/2021-05-30 22\_37\_52-Window.png>)

{% hint style="info" %}
Consult the [Netbeans platform online doc](https://netbeans.apache.org/kb/docs/platform/) for more information about the Netbeans platform API.
{% endhint %}

### Action code

The 2 most important methods are:

* **selectionChange**(), which is called each time selection has changed (e.g. user has selected or unselected bars/chord symbols/sections) with a selection context parameter. This is used to enable or disable the action depending on the selection.
* **actionPerformed()**, which performs the action.&#x20;

The automatic selection change mechanism in the active Chord leadsheet editor is provided by the **CL\_ContextActionSupport **helper class.  This mechanism is based on the powerful **Netbeans global Lookup** mechanism, which is a out of scope of this simple tutorial -but you will easily find explanations on the web.&#x20;

Below is a sketch of a possible Reharmonize action implementation.

```java
    @Override
    public void selectionChange(CL_SelectionUtilities selection)
    {
        // Action is enabled if at least 2 chord symbols are selected
        setEnabled(selection.getSelectedChordSymbols().size() >= 2);
    }
 
  
   
     @Override
    public void actionPerformed(ActionEvent e)
    {
        // Get current selection context: at least 2 chord symbols because of our selectionChange() implementation
        CL_SelectionUtilities selection = cap.getSelection();


        // Compute and show user possible reharmonizations
        // Return the user-selected reharmonization, or null
        List<CLI_ChordSymbol> newChordSymbols= getReharmonizedChordSymbols(selection.getSelectedChordSymbols());


        if (newChordSymbols != null)
        {

            // Start an undoable action which will collect the individual undoable edits
            ChordLeadSheet cls = selection.getChordLeadSheet();
            JJazzUndoManagerFinder.getDefault().get(cls).startCEdit(undoText);

            
            // Remove existing chord symbols
            for (CLI_ChordSymbol cliCs : selection.getSelectedChordSymbols())
            {
                cls.removeItem(cliCs); // Generate one or more undoable edits
            }

            
            // Replace with the new chord symbols
            for (CLI_ChordSymbol cliCs : newChordSymbols)
            {
                cls.addItem(cliCs);    // Generate one or more undoable edits
            }


            // End the undoable action: action can now be undone by user via the undo/redo UI
            JJazzUndoManagerFinder.getDefault().get(cls).endCEdit(undoText);         
                                                
        }        
               
    }
```

























