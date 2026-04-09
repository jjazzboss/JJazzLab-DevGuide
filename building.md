---
description: >-
  Creating your first JJazzLab Module! Some familiarity with Java and Maven is
  expected.
---

# Tutorial (needs to be updated for 5.1)

JJazzLab is based on the [Apache Netbeans Platform](https://netbeans.apache.org/tutorial/main/kb/docs/platform/). It provides a reliable and extensible architecture for desktop application which manages the application life cycle, the window system, extension points, options, actions, etc.

Each distinct feature in a Netbeans Platform application can be provided by a distinct Netbeans module, which is comparable to a plugin. A Netbeans module is a group of Java classes that provides an application with a specific feature.&#x20;

Unless you want to fix the JJazzLab code itself, you will probably start by creating your own Netbeans module.

## Create a Netbeans module

{% hint style="info" %}
In this example we want to add a new feature which proposes re-harmonized chord progressions. The reharmonization action will operate on the chord symbols selected by the user, and should be accessible via the chord symbol popup menu.
{% endhint %}

Let's first create a new module to hold our code. In the Netbeans IDE, follow these steps:

1. Open `JJazzLab Parent` and expand its contents
2. Right click `JJazzLab Parent / Modules` and use `Create New Module...`&#x20;
3. Select `Java with Maven` as category and `Netbeans Module` as project
4. Enter a name for the module, this guide uses "Reharmonize"
5. Click next and finish in the next step

You new module will appear in the JJazzLab modules list, but Netbeans will also open the module. If you expand all the module's contents you should have something like this:

![](<.gitbook/assets/01_new module CUT.png>)

{% hint style="info" %}
If the module did not open when created, go to `JJazzLab Parent / Modules` and locate the new module (it should be last) and either double click it, or right click and select `Open Project` .
{% endhint %}

## Create an action

We want to create a "Reharmonize" action which should be callable from the chord symbol popup menu, and should operate on the chord symbols currently selected.

The best way to start is to look for a similar action and copy the code. Actions classes are mainly found in the following modules:

* [CL\_Editor](https://github.com/jjazzboss/JJazzLab/tree/master/app/CL_Editor) module : Chord Leadsheet editor actions such as insert bar, transpose chord symbols, etc
* [SS\_Editor ](https://github.com/jjazzboss/JJazzLab/tree/master/app/SS_Editor)module : Song Structure editor actions such as delete song part, change rhythm, etc
* [MixConsole ](https://github.com/jjazzboss/JJazzLab/tree/master/app/MixConsole)module : save default rhythm mix, mute all, etc
* [MusicControlActions ](https://github.com/jjazzboss/JJazzLab/tree/master/app/MusicControlActions)module: play, stop, etc
* [SongEditorManager ](https://github.com/jjazzboss/JJazzLab/tree/master/app/SongEditorManager)module: new song, open song, duplicate song, etc

The chord leadsheet editor action [TransposeDown](https://github.com/jjazzboss/JJazzLab/blob/master/app/CL_EditorImpl/src/main/java/org/jjazz/cl_editorimpl/actions/TransposeDown.java) is enabled when user has selected one or more chord symbols, which makes it perfect to base our code on. Let's reuse it in our module:

1. Open the **CL\_EditorImpl** module, navigate to **Source Packages > org.jjazz.cl\_editorimpl.actions** and copy **TransposeDown.java** (copy the file in the projects panel, not its contents!)
2. Open the **Reharmonize** module, select the **org.myself.reharmonize** package, and choose **Paste > Refactor Copy...** (_Ctrl+V_ triggers _Refactor Copy_ in Netbeans)
3. A dialog appears, set the new name to **Reharmonize** and press Refactor

The new _Reharmonize_ class will be created in the module, but it will show many errors due to missing dependencies.

![](<.gitbook/assets/05_class_errors CUT.png>)

There are several ways of fixing the dependencies, for brevity let's just copy the ones from _CL\_EditorImpl_:

1. Go to _Reharmonize > Project Files_, open the _pom.xml_ file and delete the whole _\<dependencies>_ element
2. Go to _CL\_EditorImpl > Project Files_, open the _pom.xml_ file and copy the entire _\<dependencies>_ element
3. Switch back to the _pom.xml_ from the _Reharmonize_ module, paste the  _\<dependencies>_ element after the _\<build>_ element and save the file

Open the _Reharmonize_ class again or switch back to that tab in the editor. The errors in the import statements should be gone now! Not all problems are solved yet though...

### Action annotations

Now you should have only one error in the file:

![](<.gitbook/assets/06_action_error CUT.png>)

Netbeans uses annotations to facilitate action declaration. There are separate annotations to regulate different aspects of how the action will integrate with the application:

* [_@ActionID_](https://bits.netbeans.org/26/javadoc/org-openide-awt/org/openide/awt/ActionID.html) is used to identify the action, its required properties _category & id_ are used to locate this action from other modules (e.g. see [Actions#forID](https://bits.netbeans.org/26/javadoc/org-openide-awt/org/openide/awt/Actions.html#forID\(java.lang.String,java.lang.String\)))
* [_@ActionRegistration_](https://bits.netbeans.org/26/javadoc/org-openide-awt/org/openide/awt/ActionRegistration.html) registers the action, making it available to other modules. The _displayName_ property determines the text this action will show in the UI. If that name starts with a `#` symbolit means the actual text will be obtained from a _Bundle.properties_ localization file instead
* [_@ActionReference_](https://bits.netbeans.org/26/javadoc/org-openide-awt/org/openide/awt/ActionReference.html) puts a reference to this action in the _Netbeans virtual file system_ (created at runtime), in the _Actions/ChordSymbol_ directory. When user right clicks a chord symbol, _JJazzLab_ uses all action references in that directory to create the related menu entries, using the _position_ value to order them

{% hint style="info" %}
Check the [Netbeans Platform online documentation](https://netbeans.apache.org/kb/docs/platform/) for more information
{% endhint %}

You only need a few changes to correctly show the action, most of them in the Java code. A few changes are also required in other files.

The necessary changes for **Reharmonize.java** are:

1. In the _@ActionID_ declaration, replace the **id** by _org.myself.reharmonize_ (or a string of your choice)
2. In the _@ActionRegistration_, change the **displayName** to #_CTL\_TransposeDown_
3. There should be a use of the String literal _"CTL\_TransposeDown"_, as a parameter to [ResUtil#getString](https://www.jjazzlab.org/javadoc/org/jjazz/utilities/api/resutil#getString\(java.lang.Class,java.lang.String,java.lang.Object...\)). Change it to _"CTL\_TransposeDown"_ (notice there's no `#` at the start for this use)
4. In _@ActionReference,_ change the value of **position** from _410_ to _415_, this way the new action will appear in the popup menu after the _TransposeDown_ action.

A couple of changes are needed in other files:

1. Edit **Reharmonize > Other Sources > src/main/resources > org.jjazzlab.reharmonize > Bundle.properties** by adding a line with _CTL\_Reharmonize=Reharmonize chord progression_
2. Open **JJazzLab App > Project Files > pom.xml** and add a dependency on the new module. Go to the end of the _\<dependencies>_ and add:

```xml
<!-- Custom plugins ================================================ -->
<dependency> 
    <groupId>org.jjazzlab.customplugins</groupId> 
    <artifactId>reharmonize</artifactId> 
    <version>${project.version}</version> 
</dependency>
```

You should be able to build the new module. Right click the **Reharmonize** module then **Build** from the popup menu. Run _JJazzLab_ by right clicking **JJazzLab App** and selecting **Run** from the menu.

_JJazzLab_ will open, running with the new module. In any song (open one or create a new one), select a chord symbol and show the popup menu: our action should be there, as shown below. If you select it it will transpose down the chords symbols, as we have not yet changed the action code itself.

![](<.gitbook/assets/2021-05-30 22_37_52-Window.png>)

### Action code

The two important methods for this guide are:

* **selectionChange**() which is called each time selection has changed (e.g. user has selected or unselected bars/chord symbols/sections) with a selection context parameter. This is used to enable or disable the action depending on the selection
* **actionPerformed()**, which performs the action. This can be triggered in several ways

The automatic selection change mechanism in the active Chord leadsheet editor is provided by the **CL\_ContextActionSupport** helper class. This mechanism is based on the powerful **Netbeans global Lookup** mechanism, which is a out of scope of this tutorial.

{% hint style="info" %}
For more information on the Lookup Mechanism, [see this DZone article ](https://dzone.com/articles/netbeans-lookups-explained)recommended in the Netbeans Platform documentation
{% endhint %}

Below is a sketch of a possible Reharmonize action implementation. If you are a music enthusiast, you probably know reharmonization is a complex topic, the strategy used in this code is a risky one: replace the selected chords by randomly generated chords!

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

    // Compute a possible reharmonization
    List<CLI_ChordSymbol> newChordSymbols = getReharmonizedChordSymbols(selection.getSelectedChordSymbols());

    // Start an undoable action which will collect the individual undoable edits
    ChordLeadSheet cls = selection.getChordLeadSheet();
    JJazzUndoManagerFinder.getDefault().get(cls).startCEdit(undoText);

    // Remove existing chord symbols
    selection.getSelectedChordSymbols().forEach(cliCs -> cls.removeItem(cliCs));

    // Add the new chord symbols to replace the old ones
    newChordSymbols.forEach(newCliCs -> cls.addItem(newCliCs));

    // End the undoable action: action can now be undone by user via the undo/redo UI
    JJazzUndoManagerFinder.getDefault().get(cls).endCEdit(undoText);         
}

List<CLI_ChordSymbol> getReharmonizedChordSymbols(List<CLI_ChordSymbol> selectedChords)
{
    CLI_Factory cliFactory = CLI_Factory.getDefault();

    return selectedChords.stream()
        .map(cliCs -> cliFactory.createChordSymbol(
            ExtChordSymbol.createRandomChordSymbol(), cliCs.getPosition()
        )).collect(Collectors.toList());
}
```

Although there is a non-zero chance of this code offering a good reharmonization, the strategy is not recommended at all! A proper implementation is outside the scope of this tutorial.

{% hint style="success" %}
As previously, you can **right click > Build** the new module, then **right click > Run** on _JJazzLab App_. Select multiple chords, use _Reharmonize chord progression_ and see what you get!
{% endhint %}

Now you know:

* How to create a **new module** and add it to _JJazzLab_
* How to add **new actions** (and where to look for examples of existing actions)

{% hint style="info" %}
For more information on other internals of JJazzLab development, see the [API documentation](/broken/pages/-MQYebipasL9h3UJfoUM)
{% endhint %}
