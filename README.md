# Bash Profile in JavaScript

Easy to customize, built on top of the power of JavaScript and Bash, it adds a bunch of _aliases_, functions, features and extra funcionality for your bash profile.  
The perfect tool to optimize the JavaScript developer command line.

![termtools themes](media/termtools-built-in-themes.png)
![NPM Scripts auto completion](media/npm-scripts-auto-complete.gif)
![Termtools with battery, time and read only](media/termtools-theme-default-with-battery.jpg)

  > Still in alpha version. Looking for tests and feedback :)

## Features

- [x] Fully customizable using **JavaScript**
- [x] Applies to PS1
- [x] Adds auto completion to npm scripts
- [x] Terminal comands to enable or disable it (restoring your previous PS1)
- [x] Allows you to dinamically _turn on_ and _off_ parts of PS1
- [x] PS2 with line numbers
- [x] Auto completes git commands
- [x] Shows current **branch** and git **state** (also customizable)
- [x] Lots of Extra **aliases** (check the aliases section for more info about it)
- [x] Extra **functions**
- [x] Suport **themes** (coming with 6 themes for you to extend and customize...[see below](#themes))
- [x] Move easily from one theme to another
- [x] **Protects** some actions (like deleting or change permissions to root path)
- [x] Auto installs **fonts** for you (although, you might need to select them in your terminal settings)
- [x] Ensures colors...everywhere... _grep_, _git_, _ls_...
- [x] More tools, like time, battery and readOnly...
- [x] Extendable...you can customize your theme with any extra string, allowing you to use JavaScript to decide what to show
- [x] Create aditional, customizable parts for _$PS1_
- [x] Ignore duplicate commands in history
- [x] New terminal windows will have the history from brevious bash

## Installing it

Easy like sunday morning!

```
npm install -g @nasc/termtools
```

## Applying it

Run this command and, if everything went well, your terminal should be good looking by now!

```
termtools apply
```

This will also install the fonts you will need, if they are not already there.

## Oh Oh!

Seing _weird characters_? No worries, follow the tips your own terminal will give you.  
At any time, you can run `termtools check` to validate the characters and some colors.

The font we are using (and was already installed for you) is:  
**"Droid Sans Mono for Powerline Plus Nerd File Types Mono"**

All you gotta do is go to your terminal settings and edit your profile changing its font face/family to that one.

In _Visual Studio Code_, you can add the settings for the integrated terminal (ctrl/cmd+",", digite "terminal.integrated.fontFamily" to find it easily):

```
"terminal.integrated.fontFamily": "Droid Sans Mono for Powerline Plus Nerd File Types Mono"
```

You should be able to run ` termtools check ` and see this message:

![Termtools character set test](media/character-set-check.png)

## Removing it (restore)

Want to see your PS1 as it was before (will also loose all the _aliases_ and extra functions we had applied to your bash).

```sh
 termtools remove 
 # or
 termtools restore 
```

To bring it back, just run the ` apply ` command again:

```
 termtools apply 
```

## Reloading it

You will probably not need to reload it anytime soon, but just in case...  
After installed and applied, you have three ways to reload it. They will reload the whole bash profile (applying any updates that might be outdated).

```sh
# alternative 1
termtools reload
#alternative 2
reload
# alternative 3
termtools restore
termtools apply
```

## CLI options

You can use the _cli options_ following the pattern:

`termtools [options]`

| Option    | Description                                                                 |
|-----------|-----------------------------------------------------------------------------|
| help      | Shows the help contenbt                                                     |
| apply     | Applies the termtools PS1 effects                                           |
| reload    | Reloads the bash profile                                                    |
| restore   | Disables termtools effects, restoring your PS1 to what is was before        |
| customize | Will copy the default theme into your _home_ directory for you to customize it.<br/>**BE CAREFUL**: It will overwrite your `~/.bash_profile.js` if it already exists and you may loose any customization you had applied to it            |
| set theme | Replaces the current theme with an existing one. Use `[tab]` to see the list of installed themes.<br/>**BE CAREFUL**: It will overwrite your `~/.bash_profile.js` if it already exists and you may loose any customization you had applied to it       |
| check     | Shows a test block for you to verify if your font family is working ok      |

> You can hit [tab] for some auto-complete

## Git integration

If you are navigating in a directory that happens to belong to a Git Repository, you will see its current branch in your terminal.  
Also, the color indicates the current status of your branch and you might see symbols identifying your branch as _behind_, _ahead_ or _diverged_.

![Termtools default theme](media/termtools-theme-default-git.png)

### PS2

We also change your PS2 a little, adding line numbers for your multiple lined commands:

![Termtools multiline commands with line numbers](media/termtools-theme-default-multi-line.png)

#### Themes

Yes, we deliver _termtools_ with 6 builtin themes, they are:

- basic
- default
- hell
- sea
- pinkish
- round

You can easily move from one theme to another using the command

```

 termtools set theme [theme-name]
 
```

Just be careful! It will **replace** your ` ~/.bash_profile.js ` and, if you have done any customization to it, you will loose them.

  > If you have created a very nice theme and want to share, send us a pull request 😊

## Customizing it

You can customize _Termtools_ using **JavaScript** \o/  
And it is not even a _JSON_, nope...it is JavaScript, indeed 🙏.

We can create a boilerplate for you to customize (a copy of the default theme).  
Just run:

```
 termtools customize
```

It will create a file at **` ~/.bash_profile.js `**.  
That file is a copy of our _default_ theme, with comments and all you might need to extend it.  
This JavaScript file **must** export a _literal object_, or a _function that returns a literal object_.

If you exported a function, it will be called receiving one parameter, an object with these properties:

| Property    | Description                                                                           |
|-------------|:--------------------------------------------------------------------------------------|
| IS_TTY      | True if current session is running on a TTY environment                               |
| IS_ROOT     | True if the current user is root                                                      |
| IP          | The current device's ip                                                               |
| BATTERY     | The current percentage of the battery (give or take...some OSs lie a little about it) |
| IS_CHARGING | True if the device is connected and charging                                          |
| GIT_STATUS  | The repository status. May be from -2 to 5, meaning:<br/>-2: COMMITS DIVERGED<br/>-1: COMMITS BEHIND<br/>0: NO CHANGES<br/>1: COMMITS AHEAD<br/>2: UNTRACKED CHANGES<br/>3: CHANGES TO BE COMMITTED<br/>4: LOCAL AND UNTRACKED CHANGES<br/>5: LOCAL CHANGES |
| GIT_SYMBOL  | A symbol representing the current position of the branch. Symbols can be: <br/>"-": COMMITS BEHIND<br/>"+": COMMITS AHEAD<br/>"!": COMMITS DIVERGED<br/>"*": UNTRACKED<br/>"": Anything else |
| GIT_BRANCH  | The name of the current git branch                                                    |
| IS_WRITABLE | True if the current user has write access to the current directory                    |
| colors      | A referece to the a `chalk` instance, allowing you to add colors if you need to       |

Use these data to decide how your exported object will be. You can use it, for example, to enable or disable parts of the ` $PS1 `, or to show some parts in different colors.

Check the documentation bellow to understand it better, how to customize your terminal using JavaScript.

  > After any change you make in your customized theme, you should see the difference just by hitting [ENTER] in your terminal.  
  If not...you can force it to reload using ` termtools reload ` or just the alias ` reload `.

### Customization options

You will export a _literal object_ containing these options, or a function that returns such an object.
You can extend a given theme, or the default theme will be used.

```js
{
    extends: 'basic'
}
```

While the default theme will have a _PS1_ like the second image in this documentation, the basic theme will look like this:

![Termtools basic theme](media/termtools-theme-basic.jpg)

#### Completition (auto complete)

We will also add auto-complete for your _git_ commands, and add a richer auto-complete for your _npm_ commands as well.  
For example, we can hint all the branch names of your repository, or all the scripts from your package.json.

#### aliases

An object containing the _command_ as the key, and the _instruction_ as the value.  
For example:

```js
{
    aliases: {
        foo: "echo bar"
    }
}
```

Will then, allow you to run in your terminal:

```sh
$ foo
bar
```

Some useful aliases to add, are telated to the the environment you use to work.  
For example, let's say you keep all your projects under `~/projects/web/`, you can create a alis for going there:

```js
{
    aliases: {
        www: "~/projects/web/"
    }
}
```

Now, you can type ` www ` to go there.

#### Decorators

This will allow you to customize some of the decorators we will use in your PS1.  
So far, they are:

- pathSeparator
- section
- readOnly
- git

You can use the code (`\uCODE`) for the following characters (available in the installed font).  
For example, the code "e0a0" can be used as `"\ue0a0"`:

![Termtool fontforge](media/fontforge.png)  
(imported from _powerline nerd fonts plus_)

Also, some other symbols and code you might find promising:

![Termtools extra symbols](media/termtools-extra-useful-symbols.png)

#### ps1

This is the part where you specify the rules for your PS1.  
It has two customization options: ` parts ` and ` effects `.  
The effects are the style rules, applied for each part.

##### Parts

Every part of your PS1 has the ` enabled ` flag, allowing you to turn them on or off as you will.  
Besides that, all the properties also accept a ` wrapper `, which is a string with a "$1" in it.  
For example, if in your "username" part, the wrapper is "[$1]", it will render "[felipe]" for a user named "felipe".  
Some parts have their own special properties.

You can create any other part, and it may have the ` enabled `, ` wrapper ` and ` content ` properties (like _string_ parts). And yes, you can then customize them with effects as well.

The available parts and their special attributes are:

| Part name | Description                                                                 | Extra options |
|-----------|-----------------------------------------------------------------------------|---------------|
| battery   | Shows the current battery state                                             | N/A           |
| time      | The current time                                                            | N/A           |
| userName  | The currently logged user                                                   | N/A           |
| string    | Any given string you might wanna add                                        | content: The content of the string |
| machine   | The machine name                                                            | N/A           |
| path      | The current path (without basename)                                         | *Options escribed bellow |
| basename  | The current basename                                                        | N/A           |
| git       | If the current directory is a repository, show the git information about it | N/A           |
| entry     | The last character waiting for the user entry. Usually a "$" sign           | content: A given string for it |
| os        | The current OS                                                              |               |
| readOnly  | Shown when the current directory is readonly for the current user           | N/A           |
| custom    | your own                                                                    | content: The string to be the content |

The **path** part is special and has some very useful extra options:  

| Option     | Description                                                                | Values        |
|------------|----------------------------------------------------------------------------|---------------|
| ellipsis | Uses "…" to truncate the name of each directory in the path| ` false ` or a ` Number `, limiting the size to be ellipsed |
| cut| Will cut/truncate part of the path, ensuring it will stay inside _maxlength_. If it was truncated, "…" will be used | One of `false`, `"left"`, `"right"` or `"center"` |
| maxLength | The maximun size of the while path, is _cut_ is enabled | ` Number ` |

##### Effects

For each part you used, you can apply effects.  
The available effects are:

| Option     | Description                                                                |
|------------|----------------------------------------------------------------------------|
| color*     | The text color                                                             |
| bgColor*   | The background color                                                       |
| bold       | Sets text as bold                                                          |
| italic     | Tries to set the text as italic (not all terminals support it)             |
| underline  | Underlines the text                                                        |
| dim        | Sets the text as dim                                                       |
| separator  | By default, will be the decorator you set as separator. If ` false `, no separator will be used for that part. Can be used to customize the separator of one specific part of ` PS1 ` |

  > Values for both ` color ` and ` bgColor ` accept the colors from [chalk](https://github.com/chalk/chalk). You can also use _RGB_ colors starting with "#", for example `#f00`. But keep in mind that some hex values are not supported in some terminals.

### Extending

It's javascript! So...you can extend parts like this, for example:

```js
let osType = require('os').type().toLowerCase()
const OS_TYPE = osType == 'linux' ? '\ue712' : osType == 'darwin' ? '\ue711' : '\ue70f'

// ....

module.exports = function (data) {
    // ....
    parts: {
        customOS: { enabled: true, content: OS_TYPE, wrapper: '$1 ' }
    }
    // ....
}
```

And the results would be one of:

![Termtools extending with OS](media/termtools-extension-os.jpg)

  > Just a heads up...we do have an `os` part already, that was just an example

### Aliases

| Alias              | Description                                                                           |
|--------------------|---------------------------------------------------------------------------------------|
| fixcamera          | Fixes the camera when it is not loading (a known bug triggered in Google Chrome)      |
| ipin               | Shows the internal IP addess                                                          |
| ipout              | Shows the IP facing the public network                                                |
| ip/ips             | Shows both internal and external IPs                                                  |
| aliases            | Shows the list of currently supported aliases                                         |
| back               | Goes to the last path where you were                                                  |
| ..                 | Equivalent to `cd ..`                                                                 |
| cd..               | Equivalent to `cd ..`                                                                 |
| .2                 | Equivalent to `cd ../..`                                                              |
| .3                 | Equivalent to `cd ../../..`                                                           |
| .4                 | Equivalent to `cd ../../../..`                                                        |
| .5                 | Equivalent to `cd ../../../../..`                                                     |
| .6                 | Equivalent to `cd ../../../../../..`                                                  |
| .7                 | Equivalent to `cd ../../../../../../..`                                               |
| ll                 | A better listing of your files and directories                                        |
| ~:                 | Goes to your HOME directory                                                           |
| root               | Goes to your root path (/)                                                            |
| www                | Goes to `/var/www/`                                                                   |
| commit             | `git commit -a`                                                                       |
| commitAll          | `git add -A; git commit`                                                              |
| gitlog             | Shows a more readable log for your git repo                                           |
| gittree            | Shows a readable tree for your git repo                                               |
| checkout           | `git checkout`. Used as `checkout mybranch`.                                          |
| push               | `git push origin`. Use it like `push master`.                                         |
| pull               | `git pull origin`                                                                     |
| sizes              | Shows the size of your files and directories                                          |
| flushDNS           | Flushes the DNSs                                                                      |
| DSFiles_removal    | Removes all the `.DS_Store` files (recursivelly) in the current tree                  |
| hosts_editir       | Opens and editor for your hosts file                                                  |
| ff/findfile        | Searches for a file with the given name                                               |
| fd/finddir         | Searches for a directory with the given name                                          |
| h                  | Shows the bash history                                                                |
| lh                 | Lists only the hidden files                                                           |
| today              | Shows the date for today                                                              |
| now                | Shows the current time                                                                |
| ports              | Shows the currently pened ports                                                       |
| lsd                | Equivalent to `ls` but showing only directories                                       |
| hide-desktop-icons | Hide all the desktop icons (specially useful when presenting to an audience)          |
| show-desktop-icons | show all desktop icons                                                                |
| chromekill         | Kills all Google Chrome tabs to free some memory                                      |
| afk                | Locks the screen, as you are Away From Keyboard                                       |
| path               | Shows all the address in your `$PATH`, each one in a different line                   |
| show-hidden-files  | Show hidden files (MacOS only)                                                        |
| hide-hidden-files  | Hide hidden files (MacOS only)                                                        |
| ifactive           | Shows all the active network connections                                              |
| amioffline         | Answers "Yes" if you are offline, and "No" otherwise                                  |
| amionline          | Answers "Yes" if you are online, and "No" otherwise                                   |
| desktop/desk       | Equivalent to `cd ~/Desktop`                                                          |
| docs/d/documents   | Equivalent to `cd ~/Documents`                                                        |
| downloads/down     | Equivalent to `cd ~/Downloads`                                                        |
| emptytrash         | Empty the Trash on all mounted volumes and the main HDD                               |
| pubkey             | Copies your public key to clipboard                                                   |

### Functions

| Function           | Description                                                                           | Options |
|--------------------|---------------------------------------------------------------------------------------|-----------|
| try                | Will try the following command and if it is not installed, will show a nicer message exiting with `0`..<br/>Ex:<br/>`try some command` <br/>or<br/>`try ls -la`<br/>.  | `-q`: Executes the command if the program is installed, and does nothing if not(return value is 0)<br/>`-e`: Returns 1 as the _return value_ in case the program is not installed<br/>`-h`: A full help description of the function |
| bold               | Echoes the following arguments in bold | |
| line               | Writes a line (-) in terminal                                                         | `""`: A string with one character to be used to draw the line.<br/>Careful when using special characters as they will **require** quotes          |
| doubleline         | Writes a double line (=) in terminal                                                  |           |
| dog                | Just like cat, but paginated and using line numbers                                   | Any argument you would use with `cat`          |
| sizeof             | Gives you the size of a file, or the total size of a directory                        | `[path]`: You can pass the path for the a file or directory            |
| hierarchy          | Shows a tree of files ignoring `node_modules` and other temp files, using line <br/>numbers, pages and colors. | Any extra argument you can pass into `tree` |
| extract            | Extracts any compressed files (works with any file with <br/>extension tar.bz2, tar.gz, bz2, rar, gz, tar, tbz2, tgz, zip, Z, 7z)                  |
| pid                | Shows the PID for a given process name                                                | `process name`: You can a string or a regex, like `pid /^node$/`  |
| about              | Shows info on the current serve/session/user                                          | |
| targz              | Create a .tar.gz archive, using `zopfli`, `pigz` or `gzip` for compression            | `path`: The path for the file to be compressed |
| googl/short        | Shortens a URL using goo.gl service                                                   | `path`: The path for the file to be compressed`path`: The path for the file to be shrinked |
| datauri            | Create a data URI from a file and copy it to the pasteboard                           | `path`: The file to be copied as base64 |

### Safety

We will also add some features for safety, like:

- `rm` is now `rm -i` by default and will ask you if you are sure you want to remove a file (unless you use `-f`)
- `chgrp` will ensure you keep your root path safe
- `chown` will ensure you keep your root path safe

### License

We use an MIT license, you can find it in our [repository](https://github.com/NascHQ/termtools/).

### Code of Conduct

As everything else [Nasc](https://nasc.io/) does, we follow a _Code of Conduct_.  
Please refer to it in our [repository](https://github.com/NascHQ/termtools/).

### Have a question or suggestion?

Ask your questions in our issues with the title starting with "[QUESTION]".  
Be sure your searched for similar issues that might also have been already closed by then.  
Send suggestions opening issues with the title starting with "[SUGGESTION]".

### Influences

We noticed many well known developers share some of their aliases and customizations for terminal.  
This has inspired us as well, bringing some of the best ideas to termtools.  
Feel free to submit new ideas as well, by using the issues in this repository.

Some ideas for the aliases and functions were inspired by (or brought from):

- [@addyosmani](https://github.com/addyosmani) (Addy Osmani)
  [https://github.com/addyosmani/dotfiles](https://github.com/addyosmani/dotfiles)
- [@alrra](https://github.com/alrra) (Cãtãlin Mariş)
  [https://github.com/alrra/dotfiles](https://github.com/alrra/dotfiles)
- [@cowboy](https://github.com/cowboy) (Ben Alman)
  [https://github.com/cowboy/dotfiles](https://github.com/cowboy/dotfiles)
- [@mathiasbynens](https://github.com/mathiasbynens) (Mathias Bynens)
  [https://github.com/mathiasbynens/dotfiles](https://github.com/mathiasbynens/dotfiles)
- [@necolas](https://github.com/necolas) (Nicolas Gallagher)
  [https://github.com/necolas/dotfiles](https://github.com/necolas/dotfiles)
- [@paulirish](https://github.com/paulirish) (Paul Irish)
  [https://github.com/paulirish/dotfiles](https://github.com/paulirish/dotfiles)

### Contribute

We are welcoming new themes and all the help we might get.  
Let's get in touch :)  
