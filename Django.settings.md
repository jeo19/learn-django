## Introduction

[VS Code](https://code.visualstudio.com/) is an editor developed by Microsoft. I have been using this editor for Python development for a few months now. Previously I have been using Pycharm Community Edition for development, but I had to switch to an editor which was less resource consuming than Pycharm, hence I found VS Code.

It was initially suggested to my by one of my colleagues. My first impression was, what is this? Is it really usable? Is it as bad as Atom?(I have a dreadful experience with Atom, although Atom is maybe as good as VS Code). But instead, I found that it is really useful, user-friendly and has lots of useful features.

Let us check out, how we can use VS Code to develop Production grade Python applications.

## Configuring VS Code

### Configuring Python

For using pythonic features, you need to install plugin [Python VS Code](https://github.com/Microsoft/vscode-python) To do that, go to Extensions Section of VS Code(marked blue in the image given below), in search section, type python; and install the red marked package called **Python by Don Jayamanne(Now Maintained by Microsoft)**.

![Python](https://github.com/ruddra/blog-images/raw/master/vscode/1.gif)

Now go to **preference>settings** and open **workspace settings**: ![Image](https://github.com/ruddra/blog-images/raw/master/vscode/2.png)

Here configure the virtual environment like this:

    {
        "python.pythonPath": "/path/to/virtualenv/bin/python",
    }

That should be enough to let you use the virtual environment for development.

After that, let us add some more features which are useful to develop python codes.

### Using PEP8 and Lint

To add them to vs code, add the following key values to above dictionary:

    "python.linting.pep8Enabled": true,
    "python.linting.pylintPath": "/path/to/virtualenv/bin/pylint",
    "python.linting.pylintArgs": [
            "--load-plugins",
            "pylint_django"
        ],
    "python.linting.pylintEnabled": true

To use the above features, editor will prompt you to install **pylint** and **autopep8**, or you can install them directly in virtual environment:

    pip install autopep8
    pip install pylint

### Format on Save

Add this like in dict: `"editor.formatOnSave": true` It will auto format code (language does not matter).

### Add Ruler in Editor

Adding rulers in editor gives you a better idea of how many words will you put on a single line, in Pep8 Standard, it’s 79\. So let’s add the following key and values in the settings dictionary:

    "editor.rulers": [
            80,
            120
        ],

### Ignoring Unnecessary Files

To ignore unnecessary files, add this following lines:

    "files.exclude": {
            "**/.git": true,
            "**/.svn": true,
            "**/.hg": true,
            "**/CVS": true,
            "**/.DS_Store": true,
            ".vscode": true,
            "**/*.pyc": true
        },

### Disable Preview

When you open a file using an import file or try to go back to the daclaration of the code, vs code intends to open it in a preview window, which sometimes is annoying when you want to do it multiple steps/times. To disable it, add this:

    "workbench.editor.enablePreview": false,

## Debugging

It is really cool to have debugging feature built-in VS Code. Although as far as I tested, it works perfectly fine on Ubuntu, but not in OSX. So please check before you configure it.

Anyways, the best way to configure it for **Django** is to add the following lines in **launch.js**:

    {
        "version": "0.2.0",
        "configurations": [
            {
                "name": "Django",
                "type": "python",
                "request": "launch",
                "stopOnEntry": true,
                "pythonPath": "${config:python.pythonPath}",
                "program": "${workspaceRoot}/manage.py",
                "cwd": "${workspaceRoot}",
                "args": [
                    "runserver",
                    "--noreload",
                    "--nothreading"
                ],
                "env": {},
                "envFile": "${workspaceRoot}/../virtualenv",  //virtualenv pat
                "debugOptions": [
                    "WaitOnAbnormalExit",
                    "WaitOnNormalExit",
                    "RedirectOutput",
                    "DjangoDebugging"
                ]
            }
        ]
    }

Or go to Debug section(Marked green in the screenshot), click the section marked as yellow and then click add configuration.

![3](https://github.com/ruddra/blog-images/raw/master/vscode/3.gif)

Then, click on Django settings to add new Django settings for debugging.

![4](https://github.com/ruddra/blog-images/raw/master/vscode/4.gif)

## Useful plugins

1.  Use [Sync](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync) to synchronize your settings in between VS Code instances over multiple machines.
2.  You can use [Git Blame](https://marketplace.visualstudio.com/items?itemName=waderyan.gitblame) to see Git blames.
3.  [IntellijIdea Keybindings](https://github.com/k--kato/vscode-intellij-idea-keybindings)IntellijIdea Keybindings allows you to use Idea’s shortcut keys in VSCode.
4.  Use [Icons](https://marketplace.visualstudio.com/items?itemName=robertohuertasm.vscode-icons) to beautify VS Code’s icons.
