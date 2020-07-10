# Skill Flow Builder (II): Using SFB editor and Basic Concepts

In this blog we will explain how to use the Skill flow Builder Editor GUI to create, edit, and preview content for your skill. The project can be downloaded [here](https://github.com/goalu/SFB-Tutorial).

## General View

[Image: SFB_General_View.png]Once you open the SFB editor you will see a similar image as above. The main elements you will find are:

1. _*Project creations and Project history*_. Here you will be able to open or create a new project or review a recent project from the Recent Projects list.
2. _*File and View Menu*_. 
    1. *File*. Where you will be able to open, create and close projects. In addition, it has options to “*build*” the project (similar to the command  `alexa-sfb build`), “*reset*” the editor and a troubleshooting action *“clear-voice-preview-cache*” (the functionality will be explained in the project simulation section).
    2. *View*. To modify the views of the editor.
3. *_Edit and Simulate tabs_ *. Here is the main editor of your interactive story files you can recognize in your SFB project because they are stored with the extension `.abc` . From left to right you have the “*source*” (‘</>’) view where you can see the text view of the story file, “*guided*” view (compass icon) offers a graphic interface to edit the story files and “*simulate”* (play icon) that will allow you to simulate your flows. Every story content can be stored in one or multiple story files (`.abc`). These stories will follow a specific syntax and can be edited and simulated in these views.
4. _*Map and resources palette*_. This palette contains the resources (“*images*”, “*snippets*”, “*slots*”, “*audio*”) and the “*map*” (tree icon) where you can see a tree structure representing the flow nodes of your story. The details for adding nodes will be explained later.
5. *_Log views_.* It includes the different logs recorded after opening the editor. From left to right you have “*Log Messages*” recording any editor event (save, open,etc.), “*Error Messages*” containing any syntax error in our flow and “*Build Output”* containing any information related to the last “*build*” of the project.

## Creating a New Project

Go to “*File Menu” * or “*Project History” * and select “*New project*”. After clicking it you will see a screen like this:
[Image: image.png]
From all the options the most relevant is the type of  “*Starter Template*” Existing templates contain rich samples you can use to build your skill:

* _*Blank*_. Create a flow builder skill from scratch. 
* *_Example Story_*_ and _*_Adventure_*. Select one of these templates when you want to create a narrative story. This is a really useful template you can use to have a better understanding of the different SFB features.
* *_Quiz_. *This template contains all the components required to create a Quiz and it can be used to load questions defined in the story file(`.abc` ) or to load them from an external file (`.csv`). Consider this last option if you want to build skills that  can access questions from an external source.

We will start our development from scratch. Select “*Blank template*”,  write your project name and project directory,  then, click OK.

### Basic Elements of a Scene

 SFB is organized around the idea of scenes and and we can consider them as the main building block. A sce imitates a film or theatrical screenplay, in which the narrative is told through a series of scenes. In a scene, a small portion of the narrative unfolds, through characters, visuals, and sound design. Then, the players makes a decision, speaks that decision to Alexa, and the skill advances to the appropriate scene.
After creating your blank project the editor will open the “*Map* *view*” and the “*Source view”* showing your `story.abc` file.  The node or *scene* added in this blank template is a [special one](https://developer.amazon.com/en-US/docs/alexa/custom-skills/skill-flow-builder-reference.html#start):  `@start` which is the first scene for a new user to the story or after a restart of the story. 
For now we will just focus on the main elements of a scene. Select your  `start`  scene in the “*Map* *view*” and  change the view to “*Guided”*. 
[Image: image.png]
Main elements of a scene are:

* `say`: This is what Alexa says to the user when it plays the scene. This is what the narrator or character says and you can use   [Amazon Polly](https://aws.amazon.com/polly/) to support multiple voices/characters. 
* `recap`:  This is what Alexa says when the user says an unexpected response.
* `reprompt`: This is what Alexa says if the user says nothing when the scene expects a user response (“*Hear Action*”).
* `then `or navigation actions: These are the instructions or commands executed after Alexa  plays the content (`*say*`, `*recap *`or `*reprompt*`) of the scene. There are two type of actions:
    * *Go to* ( `->` ) .  Defines how the scene will transition to the next one. 
    * `*Hear*`.  It specifies the words or phrases to listen to, and what should happen if heard.

For now, let's add 2 scenes that will be connected to our `start` node. Go to the “*Map*” view, select the node and click ‘+’ (bottom right corner in the “*Map*” view), then select the new one and repeat the process. Now you should have 3 scenes connected in your map view as the below image: 
[Image: image.png]Edit the `say`, `recap` and `reprompt` on each scene, and then “S*ave*” the project.  If you prefer you can go to the “S*ource* *view”* and paste the following code:

```
@start
    *say
       Welcome to the Skill Flow Builder. 
    *then  
        -> learn scenes 

@learn scenes
    *say
        Great! Welcome to the framework! Let's get started. 
        Scenes are the basic building blocks of a story in the Skill Flow Builder.  
        They can be connected linearly, branching, looping, or any other way you can think of. 
        First, you have to give each scene a name. For example, this scene is named 'learn scenes'. This name needs to be unique within your story. 
        Then, you can give your scene some content. For example, this scene has 'say' content, which you are hearing right now and some 'then' content which tells it where to go next.       
    *then
        -> learn say and reprompt 

@learn say and reprompt
    *say
        Let's learn more about 'say' and 'reprompt' content within a scene.
        What you are hearing right now is this scene's 'say' content.
        To hear this scene's 'reprompt' try not saying anything...
    *reprompt
        'reprompt' content is what the player will hear if nothing is said or we didn't understand what the player said.
        If you don't provide 'reprompt' content, it will just repeat the 'say' content. 
        Try saying 'continue' this time.
    *then
```

### Navigation actions: Hear.

SFB's `hear` action allows the skill to select the next scene based on the user’s response. To create one, select the last scene ‘learn say and reprompt’ and go to the guided view, select “*Add hear Action*” and add the following content (it will add a new scene called ‘learn choices’ that we will use in the next exercise). Then save it.
[Image: image.png]So far we have added a `hear` action but you would probably like to add multiple choices and not just one. Go to your new  ‘learn choices’ scene and in the “G*uided view*” add two “*Hear actions*” (one should go to the scene ‘more on choices’ and the other to ‘learn variables’) . You can copy/paste the following code in the scene:

```
@learn choices
    *say
        In the 'then' content, you can tell the scene to go somewhere if it hears something by using the keyword 'hear'. 
        We've been using this in the previous scenes to listen for the word 'continue' and then -> the next scene.
        You can use 'hear' multiple times in order to give players choices.
        For example, would you like to learn more about choices or move on to the next topic?
    *then
        hear learn more about choices {
            -> more on choices
        }
            
        hear move on to the next topic {
            -> learn variables
        }
```

_*Remember*_: If the user says an utterance that is not supported in our `hear` actions, SFB will read the `reprompt` message and if it's not defined it will repeat the `say` text again. 

SFB is a graph so you can navigate to a scene from multiple nodes . Let’s try it: Go to your new scene ‘more on choices’ and add a “*Hear action*” to the scene ‘learn variables’.
After completing these steps your “*Map* *view*” should look similar to this one:
[Image: image.png]
```
@more on choices        
    *say
        Players might respond with variations of the command you provide them. You can list multiple variations inline using a comma to separate them.
        For example, say 'continue', 'move on', or 'get moving' to continue.
    *then
        hear continue, move on, get moving {
            -> learn variables
        }

@learn variables
    *say
        Great! Sometimes you might want to add or remove items or change some attributes to make your story more exciting.
        You can accomplish this in the 'then' content. 'set', 'increase', or 'decrease' a variable. 
        For example, we set 'antidote' to 0.
        Now, we can use this variable in our story. Say 'continue' to find out how.
    *then
```

### Terminators and Special Scenes

Terminators , noted as ‘`>>*`’,   are special instructions you can include in your scenes. The most important terminators are:

* `>>END`. This will end the flow and next time user restarts the skill it will go to the `@start` scene.
* `>>RESTART`. It will restart the flow and it will move to the `@start` scene. It will reset all choices (flags) but it will not reset variables.
* `>>RESUME`. It will go to the scene where the player last left off.
* `>>BACK`. It will go to the scene of the previous interaction.
* `>>REPEAT`. It replays the audio artifact that the user just heard.

SFB offers a list of special scenes you can use in your skills. Like any other scene, the scene identifier is unique so you cannot repeat it. The most relevant scenes are:

* `@start`: It' required in any SFB skill and it’s executed for all new users and after a restart.
* `@pause`: Triggered after user says ‘cancel’, ‘pause’ or ‘stop’ ([AMAZON.CancelIntent, AMAZON.PauseIntent and AMAZON.StopIntent](https://developer.amazon.com/en-US/docs/alexa/custom-skills/standard-built-in-intents.html)) or after using the special terminator `>>PAUSE`.
* `@resume`:  This scene is played when the user comes back to the skill. From this scene you can send users back to where they left off by using a `>>RESUME`
* `@global prepend`. The content of this scenes is prepended to every scene in the game. It will be executed before the current scene.
* `@global append`. The content of this scene is appended to every scene in the game. It will be executed after the current scene.


In the following exercise we will add a global help,  repeat, restart and a previous scene handler to our flow (e.g: the user can ask for help in any scene) and we will also add the special scenes `@resume`, `@pause` . 

1) Open the “*Source view*”.
2) Copy and paste the following code to add the scenes.

```
// Played before resuming the skill
@resume

// Triggered on AMAZON.CancelIntent, AMAZON.PauseIntent and AMAZON.StopIntent
@pause

// Appended to all scenes (performed after every scene)
@global append
```

3) Save the project and switch to “*Map* *view*”.  You should see something like this:
[Image: image.png]
4) Select ‘resume’ scene. Go to “*Guided view”* and add to `say` following test:

*`“This is a resuming scene. This scene is played when the user re-launches your skill after exiting. Do you want to continue your story from where you last left off?”`*

5) Add to the scene two `hear` actions (Utterance: ‘Yes’, action: → Resume; Utterance: ‘No’, action: → Restart) and save the project.
6) Select the Pause scene and add this `say` message: *`“Thanks for playing!”`*
7) Select the `@global append` scene and add the following `hear` actions:
    - Utterances:* restart, start again ;  *Action:* → Restart
    - Utterances: repeat, say again, repeat it, what did you say, I didn’t understand ;  Action: → Repeat
    - Utterances: help, I need help, please help me ;  Action: Go to Scene help *(help is a new scene)
    - Utterances: back, previous, rewind ;  Action: → Back
8) Select the new ‘Help’ scene and add this `say` message: `*“This is a help message. In this story, this scene is set up to be reachable only through the global scene.”*`
9) Add an action to the ‘Help’ scene and select ‘Back’.

If you have followed correctly all instructions you source code should include this at the end:

```
@resume
    *say
        This is a resuming scene. This scene is played when the user re-launches your skill after exiting.
        Do you want to continue your story from where you last left off?
    *then
        hear Yes {
            >> RESUME
        }
        hear No {
            >> RESTART 
        }

@pause
    *say
        Thanks for playing!

@global append
    *then
        hear restart, start again {
            // Tell the framework to restart from the beginning
            >> RESTART
        }
        hear  repeat, say again, repeat it, what did you say, I didn’t understand {
            // Tell the framework to repeat everything the player just heard. You can use >> REPROMPT which only plays the reprompt content
            >> REPEAT
        }
        hear help, I need help, please help me {
            -> help
        }
        hear back, previous, rewind {
            >> BACK
        }

@help
    *say
        This is a help message. In this story, this scene is set up to be reachable only through the global scene.
    *then
        >> BACK
```

### Collecting user values

SFB allow developers to collect and use values in their stories. These values can be simple [variables](https://en.wikipedia.org/wiki/Variable_(computer_science))representing a number, a string, a predefined enumeration (e.g: names, actors, cities, etc.) or a custom value that developers can define in the “S*lot types”* (e.g: list of fruits, player classes, etc.). 

The simplest way to capture a value is using the command `hear` and the `{slot}` value in the utterance. The `slot` instruction tells Alexa that when we are using `{slot}` for a `hear` it should expect to hear a number, string, or an enum (depending on the slot type definition). For example:

```
slot playerName as 'AMAZON.US_FIRST_NAME'
hear my name is {playerName}, {playerName}, call me {playerName} {
    -> learn scenes
}
```

The above code will tell Alexa that when a user says ‘*my name is XXX*’  it should expect a name in *XXX* from the list of available names in the AMAZON.US_FIRST_NAME type.  You can select any built-in [type](https://developer.amazon.com/en-US/docs/alexa/custom-skills/slot-type-reference.html)(be sure the type is available in your locale) or create your own type.

You can also reference your collected value by using `{slot}` notation in the `say` command. 
Go to your `start` node and select the “*Source view*”, then copy paste following code:

```
@start
    *say
       Welcome to the Skill Flow Builder. Let's get to know each other.
       What is your name?
    *then  
        slot playerName as 'AMAZON.US_FIRST_NAME'
        hear my name is {playerName}, {playerName}, call me {playerName} {
            -> learn scenes
        }
```

Now we are going to use that value in our `say`, command. Go to the ‘learn scenes’  “S*ource view”* and then replace the content with this:

```
@learn scenes
    *say
        Great! Welcome to the framework {playerName}! Let's get started.
        Scenes are the basic building blocks of a story in the Skill Flow Builder.  
        They can be connected linearly, branching, looping, or any other way you can think of. 
        First, you have to give each scene a name. For example, this scene is named 'learn scenes'. This name needs to be unique within your story. 
        Then, you can give your scene some content. For example, this scene has 'say' content, which you are hearing right now and some 'then' content which tells it where to go next.       
    *then
        -> learn say and reprompt 
```

“*Save*” and “*Build*” the project again. 
You can also add a value in the “*Guided view*” by double clicking the word (without brackets) and selecting “*Value*”.
[Image: image.png]
To create your own type you need to go to the “*Slot type view*” (icon ‘*?*’) under the “*Map view*”. Then select a “*New slot type*”:
[Image: image.png]You can now add values by clicking the ‘*+*’ button
[Image: image.png]These slot types can be used in the same way as any AMAZON built in type. 
Create a `CustomFruitType` slot and add ‘pear’, ‘pineapple’, ‘apple’, and ‘orange’ as values. 

### Variables

SFB allows developers to store information about the progress of the story, manipulate that information and make it persistent even after closing the session. Based on the type of value we can classify them in three types:

* *Boolean *values (true or false). These variables are declared and set using the commands `flag` (declare and set value = true) and `unflag` (declare and set value = false).
* *Primitive *values (numbers and strings). Used to store a string or number. They are declared using the `set` command
* *Enumerated types  *(a limited list of string values). This has already been described in previous section and they are declared using `slot` command.

You can use the variables in your `say` as explained previously but you can also use them to define navigation logic in your flow using `if`.
Go to ‘learn variables’ scene and modify the code in the “*Source view*”

```
@learn variables
    *say
        Great! Sometimes you might want to add or remove items or change some attributes to make your story more exciting.
        You can accomplish this in the 'then' content. 'set', 'increase', or 'decrease' a variable. 
        For example, we set 'antidote' to 0.
        Now, we can use this variable in our story. Say 'continue' to find out how.
    *then
        set antidote to 0

        hear continue {
            -> learn conditional
        }
```

We are declaring a numeric variable that will be initialized to 0.
Now add the scene ‘learn conditional’ with the following code:

```
@learn conditional
    *then
        if antidote is 0 {
            -> antidote zero
        }
        
        if antidote is 1 {
            -> antidote one
        }

        -> antidotes
```

This scene will verify the value of our variable `antidote` and it will send us to different scenes depending on the value (0, 1 or other). Note that in this scene we didn’t include any `say` command so in case we resume the skill right here then it will restart in the previous scene ‘learn variables’.

Now complete the code adding the snippet below. If the user leaves the game while in these scenes then the value of the variable antidotes will be kept. If we want to remove the value we need to use the command `clear`.

```
@antidote zero
    *say
        You are hearing this because we detected that you don't have any antidotes.
        We used an 'if' statement in the 'then' content to accomplish this. Use a period to close your 'if' statement.
        Add an antidote by saying 'add antidote'. 
    *reprompt
        Say 'add antidote.'
    *then
    
        hear add antidote, antidote, more antidote {
            increase antidote by 1
            -> learn conditional
        }

@antidote one
    *say
        You are hearing this because we detected that you now have 1 antidote.
        Add one more antidote by saying 'add antidote'. 
    *reprompt
        Say 'add antidote.'
    *then
        hear add antidote, antidote, more antidote {
            increase antidote by 1
            -> learn conditional
        }
    
@antidotes
    *say
        You have {antidote} antidotes. 
        You are hearing this because the first 2 'if' statements weren't triggered. 
        When an 'if' is triggered and it has a '->' in it, nothing afterwards is executed. 
        Try adding one more antidote or say continue to move to next scene.
    *recap
        You have {antidote} antidotes. 
        This is the same scene as the one you just heard but you are hearing the recap content instead of the say content.
        The 'recap' is played if you have already heard this scene once and can be useful in trimming down long scenes that the user could encounter multiple times.
        You can add even more antidotes by saying 'add antidote' or say 'continue' to move on.
    *then
        hear add antidote, antidote, more antidote {
            increase antidote by 1
            -> learn conditional
        }

        hear continue, move on {
            -> images
        }
```


The table below describes the syntax you can use in your scenes depending on the types used.

|	|Boolean	|Number	|String	|Enums	|
|---	|---	|---	|---	|---	|
|Declare	|**** [flag|unflag] <name>.***
If not declared variable will be
set to false.	|*** set <name> to <value>**	|*** set <name> as '<value>'**	|*** slot <name> as <slotType>**	|
|Modify	|* ***flag <name>*** . Set true
* ***unflag <name>*** . Set false	|* ***set <name> to <value>***
* ***increase <name> by <value>*** Increase
variable <name > by <value>
* ***decrease <name> by <value***> Decrease
variable <name > by <value>	|*** set <name> as '<value>'**	|	|
|Logical operands	|*** <name> is true
* <name> is false
* !<name>
* <name>**	|*** <name> [<|>|>=|<=|==|!=] <value>
* <name> is <value>
* !name**	|*** <name> is '<value>'**	|*** <name> is '<value>'
* !name**	|
|Say
Reprompt
Recap	|{name}	|
|Remove	|**clear <name>**	|

### **Improving Skill Engagement**

Now that we have covered the most important topics when building a SFB skill we can take a look at how to improve the skill engagement by adding images, music and voices to your skill. 
First we will use `show` which displays the visual components assigned within this section. As this is a basic tutorial we will use the `'default'` [APL](https://developer.amazon.com/en-US/docs/alexa/alexa-presentation-language/understand-apl.html)template included in the project. 
Go to  the ‘images’ scene, go to the “*Source View*” and then copy and paste the following code:

```
@images
    *say
        Skill Flow Builder allows you to show content on supported devices during a scene.
        Ready to continue?
    *show
        template: 'default'
        background:'https://m.media-amazon.com/images/G/01/alexa-games/backgrounds/memorystory-gui-1._CB473869069_.png'
        image:'https://sfb-framework.s3.amazonaws.com/examples/images/alps.jpg'
        title: 'Skill Flow Builder'
        subtitle: ''
    *then
        hear continue, yes, ready {
            -> background music
        }
```

This template will create a simple presentation with a title and a background image. 
Now we will add a background sound while Alexa reads the `say` text. To do that we will use the instruction `bgm <audiourl>` that needs to be included in the `then` section and it requires [Amazon Polly](https://aws.amazon.com/polly/)to be enabled in your skill configuration. As Polly is enabled by default in our project we don’t need to worry about it. In addition, you can also use [SSML](https://developer.amazon.com/en-US/docs/alexa/custom-skills/speech-synthesis-markup-language-ssml-reference.html)tags in the say section. 
Create a new ‘background music’ scene and add this code:

```
@background music
    *say
        Skill Flow Builder allows you to mix sounds into the background of a scene 
        and use SSML in the say section, 
        like playing a trumpet sound. <audio src='https://s3.amazonaws.com/alexa-ml/testadventureskill/en-US/audio/trumpet_1.mp3' /> 
        Ready to continue?
    *then
        // BGM requires polly-config enabled from the abcConfig.json
        bgm https://alexa-ml.s3.amazonaws.com/sounds/sound-library/EXTRA/Crowd_Cheer_2_LONG.mp3
        hear continue, yes, ready {
            -> congrats
        }
```

As final step add a last scene named ‘congrats’

```
@congrats
    *say
        Congratulations! You have finished the basic tutorial.
        There are more features that can be found in the documentation.
```

### Snippets [OPTIONAL]

Snippets allow developers to provide a shorter syntax for performing common operations in *.abc files. They can be found in the editor with the scissor symbol.
[Image: image.png]
Snippets can be used to  shorten the URLs to resources used in your skill such links to audio or image files. To use a snippet, use brackets that enclose the snippet name like [<snippetName>] in any of your skill say, recap, prompt again, or then sections.

To create a snippet:

1. click on “*New snippe*t” button
2. then name it as trumpet and add the following ssml code:
    `<audio src='https://s3.amazonaws.com/alexa-ml/sounds/sound-library-loud/Instruments/Trumpet_1.mp3' />`

[Image: image.png]
1. Then click on “*Create*”. This code will play a trumpet sound when the snippet is used.

Repeat previous steps and create following snippets:

|name	|code	|
|---	|---	|
|kid	|<voice name='Justin'>	|
|/kid	|</voice>	|
|pause	|<break time='1s'/>	|
|pause for	|<break time='$1'/>	|

Now lets use our snippets:

1. Go to the “Map View” and create a new scene * *‘snippets’,  connect previous ‘background music’ scene to this one and add a `hear action`  (yes, continue, ready) to redirect to ‘congrats’ scene.
2. Add to the `say` message this text

```
        Snippets allow you to insert content or markup into your story. 
        Here are a few common use cases for snippets.
        Sound effects [trumpet]
        [kid]
            Character voices.
        [/kid]
        Pauses [pause]
        And more! Even longer, dramatic pauses, by passing arguments to a snippet.
        [pause for 2s]
        You can define snippets in the abcconfig file.
        Ready to continue?
```

The updated code should look like this:

```
@background music
    *say
        Skill Flow Builder allows you to mix sounds into the background of a scene and use SSML in the say section, like playing a trumpet sound. <audio src='https://s3.amazonaws.com/alexa-ml/testadventureskill/en-US/audio/trumpet_1.mp3' /> 
        Ready to continue?
    *then
        // BGM requires polly-config enabled from the abcConfig.json
        bgm https://alexa-ml.s3.amazonaws.com/sounds/sound-library/EXTRA/Crowd_Cheer_2_LONG.mp3
        hear continue, yes, ready {
            -> snippets
        }

@snippets
    *say
        Snippets allow you to insert content or markup into your story. 
        Here are a few common use cases for snippets.
        Sound effects [trumpet]
        [kid]
            Character voices.
        [/kid]
        Pauses [pause]
        And more! Even longer, dramatic pauses, by passing arguments to a snippet.
        [pause for 2s]
        You can define snippets in the abcconfig file.
        Ready to continue?
    *then
        hear continue, yes, ready {
            -> congrats
        }
```

As you have noticed one of the snippets we created is using ‘`$1`’ that is the way we pass parameters (“`2s`”). This can be really useful when we want to reuse code in the .`abc` file. There is no limit of the number of parameters, just ensure you define them in your snippet code (`$1`, `$2`, `$3`, etc.)
Finally there is a predefined variable you can use in your code to substitute your skills default content URL: `$SNIPPET_S3_RESOURCES_URI`. For example, this code `<audio src='$SNIPPET_S3_RESOURCES_URI/audio/$1'/>` will be replaced during runtime/simulate by this one: `sfx": "<audio src='https://s3.amazonaws.com/alexa-ml/testadventureskill/en-US/audio/$1'/>`.
**Note:** This special variable replacement can also help with content localization of audio/images files, as the generated URL contains the locale in the generated URL.

## Simulate

We have completed our skill and we want to see how it works . Before we try it out please save your project, check there is no error message.
Then select one scene in your “*Map view*” (e.g: background music) and click the tab “*Simulate*” (play icon). You will see an image like this:
[Image: image.png]
* *Simulate from Start* will send you to the start node and you will be able to test the full flow.

* *Simulate from current* will start the simulation from the current node

* *Load state XX *will allow you to test previously tested and store scenes.

* *Voice preview* toggle will enable or disable speakers. This is quite useful if you just need to test the flow and don’t want to wait till Alexa reads the `say` text.

Select “*Simulate from Current*” (this may take several seconds till SFB prepares the code and audio) and then you will see this image:
[Image: image.png]From the “*Simulate View*” you will be able to navigate to any of the available scenes (e.g `hear` options: ‘continue’, ‘restart’, ‘repeat’, etc) by clicking one of the buttons. If you have variables and images you will also see them in the “*Simulate view*”.
[Image: image.png]If you have errors opening the “*Simulate view*” or is not refreshed go to “*File*”→“*Toubleshooting”*→“*Clear Voice Preview Cache*” and try it again.


## Conclusion

In this basic tutorial, we used the SFB editor and we learnt how to use this powerful  tool to create stories. 
You can find advanced topics in *Skill Flow Builder (III): Advanced Concepts* where I will explain more complex functionality: how to deploy the project, configuration tips & tricks and how to extend the core functionality by using extensions.

## Related Content

* [Skill Flow Builder (I)](https://quip-amazon.com/UzOZAJhJhzpo/Skill-Flow-Builder-I)
* [Skill Flow Builder Editor](https://developer.amazon.com/en-US/docs/alexa/custom-skills/use-the-skill-flow-builder-editor.html)
* [Basic Skill Flow Builder Syntax](https://developer.amazon.com/en-US/docs/alexa/custom-skills/basic-skill-flow-builder-syntax.html)
* [Advanced Skill Flow Builder Syntax](https://developer.amazon.com/en-US/docs/alexa/custom-skills/advanced-skill-flow-builder-syntax.html)
* [Skill Flow Builder Reference](https://developer.amazon.com/en-US/docs/alexa/custom-skills/skill-flow-builder-reference.html)
* [Speech Synthesis Markup Language (SSML) Reference](https://developer.amazon.com/en-US/docs/alexa/custom-skills/speech-synthesis-markup-language-ssml-reference.html)
* [Understand Alexa Presentation Language (APL)](https://developer.amazon.com/en-US/docs/alexa/alexa-presentation-language/understand-apl.html)

