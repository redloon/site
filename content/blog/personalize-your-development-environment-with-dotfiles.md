+++
categories = [ "Guides" ]
date = 2021-12-16T05:00:00.000Z
description = "With a few small tweaks, take the default terminal and make it uniquely yours - then watch your dotfiles evolve as you find new tricks for your toolkit."
draft = false
images = [ "/uploads/dotfiles-terminal-prompt.jpg" ]
tags = [ "dotfiles", "command-line", "terminal" ]
title = "Personalize your Development Environment with Dotfiles"
_template = "blog_post"
+++

Using the command line in it's default state is doable, in the same way eating plain toast 🍞 every morning will feed you. Is it enjoyable? not really. With a few small tweaks though, it can make you feel like a super hero. Dotfiles track these tweaks and encourage you to add more. We'll go over what the heck dotfiles even are (and why you should care), how to get rolling with them, and give you my favorite tweaks so you can start your own unique collection.

## What are dotfiles?

On your computer, there are hidden files called `dotfiles`, they're hidden because they start with a `'.'` e.g. `~/.bashrc` . they usually control configuration of tools or the terminal (command line) itself. People will also use the term "dotfiles" to mean a repo of someone's personalized `dotfiles`, like _"Wow, Jeff has an amazing dotfiles repo on Github!"_.

## why keep track of your dotfiles?

Since dotfiles (the actual files) store the configuration settings of your machine, by tracking them you can save the tweaks and tools you personally find handy. The best part? They grow with you over time as you find interesting tidbits on the internet. Rome wasn't built in a day, and neither was the perfect developer setup. Depending on how you use them, you can get bonus benefits:

* Documents how to fix the weird error you run into every 7 months and spend an hour searching for the command which fixed it last time - use aliases & functions. For example:

      # (╯°□°）╯︵ ┻━┻
      alias mykde_desktop_dissapeared="killall plasmashell; kstart plasmashell"
      
* Help you stop treating your machine like a [pet and start treating it like cattle](http://cloudscaling.com/blog/cloud-computing/the-history-of-pets-vs-cattle/). If your computer gets lost or stolen, you should be able to easily make the new machine feel like home with the same environment. More in [Extra Resources](#extra-resources) on automating personal machine setup.
* Gradually "automate" tasks in your life. [Do Nothing](https://blog.danslimmon.com/2019/07/15/do-nothing-scripting-the-key-to-gradual-automation/) scripts can be used for anything from code you don't yet know how to write, to manual tasks you don't want to forget.

## How to start your Dotfiles

If you want the least amount of effort, follow **Option A** with Dotbot. If you want to have a better understanding of what is happening under the hood, try **Option B**.

### Option A: using a tool

There are a variety of fantastic tools to track dotfile history, syncing, and anything else you can think of at [https://dotfiles.github.io/](https://dotfiles.github.io/ "https://dotfiles.github.io/").

> ⚠️ _The only one I'd recommend out of the list for a beginner is [Dotbot](https://github.com/anishathalye/dotbot), since it eschews most of the complexity of other dotfiles systems. No need to set up a complex syncing tool if you end up ignoring your dotfiles after you finish setting them up._

### Option B: Using a custom script

1. start a Git repo, I prefer to have all my files in the repo without the dot so it's more obvious when tracking in Git and also won't overwrite anything on accident.
2. Clone that repo to `~/dotfiles`.
3. Write a [script](https://github.com/redloon/dotfiles/blob/master/link_dotfiles.sh), or document the command, for [symlinking](https://linuxize.com/post/how-to-create-symbolic-links-in-linux-using-the-ln-command/) the real file to the dotfiles version, e.g. `ln -s ~/dotfiles/bash_profile ~/.bash_profile`.

You're off to the races! My best advice is to not try to collect everything at once or you'll end up with a pile of stuff you don't need. Let it evolve with you over time rather than aiming for perfection out of the gate.

## Favorite Tweaks

My [dotfiles](https://github.com/redloon/dotfiles) are full of tidbits I've stumbled on, so I pulled out the best pieces for you.

### Pump up the prompt

Probably the most generally useful tweak I have is fleshing out the prompt with more information. Mine includes the time it ran, the current working directory, Git branch & status, and last exit code. In text form `15:36:45:~/dotfiles:(master)* [0]▶`.

![Terminal prompt with previous exit code, current working directory, Git information, and time.](/uploads/dotfiles-terminal-prompt.jpg#center)

The exit code has saved me in a surprising number of situations from deeper debugging. You can use [https://scriptim.github.io/bash-prompt-generator/](https://dotfiles.github.io/ "https://dotfiles.github.io/") or [http://bashrcgenerator.com/](https://dotfiles.github.io/ "https://dotfiles.github.io/") to easily generate a similar setup.

    # Git Aware Prompt - shamelessly stolen from <https://github.com/jimeh/git-aware-prompt>
    find_git_branch() {
      # Based on: <http://stackoverflow.com/a/13003854/170413>
      local branch
      if branch=$(git rev-parse --abbrev-ref HEAD 2> /dev/null); then
        if [[ "$branch" == "HEAD" ]]; then
          branch='detached*'
        fi
        git_branch="($branch)"
      else
        git_branch=""
      fi
    }
    
    find_git_dirty() {
      local status=$(git status --porcelain 2> /dev/null)
      if [[ "$status" != "" ]]; then
        git_dirty='*'
      else
        git_dirty=''
      fi
    }
    PROMPT_COMMAND="find_git_branch; find_git_dirty; $PROMPT_COMMAND"
    
    export PS1="\\[\\033[38;5;11m\\]\\t:\\w:\\$git_branch\\$git_dirty\\[$(tput sgr0)\\]\\[\\033[38;5;15m\\] \\[$(tput sgr0)\\]\\[\\033[38;5;11m\\][\\[$(tput sgr0)\\]\\[\\033[38;5;7m\\]\\$?\\[$(tput sgr0)\\]\\[\\033[38;5;11m\\]]▶ \\[$(tput sgr0)\\]"
    export CLICOLOR=1
    

If you want to expand on the `git_dirty` status indicator, you can [read here](https://adamhollett.com/posts/2021/04/terminal-tricks-from-my-dotfiles/) how to make a fancy one with different shapes & colors indicating different statuses you rely on.

### Aliases

* Aliases lessen how much I have to type and also do the remembering of common args I need. You can also add functions that take arguments, though eventually they could move to a `$HOME/bin` folder or similar along your `$PATH`. You can check mine out [here](https://github.com/redloon/dotfiles/blob/master/bash_aliases). The most useful one is `extract()` so I never have to figure out how to open a compressed file - this was ripped from someone's dotfiles many years back, the origins are unknown.

### (Niche) OS-specific configuration

I work across a variety of machines - Mac, Linux, and Windows. It's nice to have a single set of dotfiles which automatically works no matter where I am using them. I have specific `bashrc` and `bash_aliases` files for each OS (this could be done with any file though). An added tweak with this to add an `echo` at the top of `bash*` dotfiles so when terminal is starting, I can tell which configuration files were used in case anything unexpected happens.

    case $OSTYPE in
        solaris*)
            echo "You have Solaris??"
        ;;
        darwin*)
            if [ -f ~/.bashrc_macos ]; then
                . ~/.bashrc_macos
            fi
        ;;
        linux*)
            if [ -f ~/.bashrc_linux ]; then
                . ~/.bashrc_linux
            fi
        ;;
        bsd*)
            echo "You have BSD??"
        ;;
        *)
        echo "Unknown OSTYPE $OSTYPE in bashrc check"
        ;;
    esac
    

These are the most generally useful tips I can provide, but that's the beauty of dotfiles. It works unique to my set up and saves me time & frustration despite it not being perfect for others.

## That's a wrap

Your terminal will be now uniquely yours, but beware! You are entering a rabbit hole of a bajillion awesome tools and scripts to try out. Enjoy the ride, but don't get too lost in trying out shiny toys that you miss out on having a reason to use them in the first place. Enjoy the adventure!

## Extra Resources

If you're feeling inspired, here's a few extra resources for the world of dotfiles and the command line:

* [Dotfiles inspiration list](https://dotfiles.github.io/inspiration/)
* [https://github.com/webpro/awesome-dotfiles](https://dotfiles.github.io/ "https://dotfiles.github.io/")[‣](https://github.com/webpro/awesome-dotfiles%E2%80%A3)
* [https://github.com/learn-anything/command-line-tools](https://dotfiles.github.io/ "https://dotfiles.github.io/")
* [https://github.com/agarrharr/awesome-cli-apps](https://dotfiles.github.io/ "https://dotfiles.github.io/")
* [How to do many common things in Bash - Pure Bash Bible](https://github.com/dylanaraps/pure-bash-bible)
* [Explanations of shell commands](https://explainshell.com/) and [human-readable man pages](https://github.com/tldr-pages/tldr)
* Beyond Dotfiles - [automating your entire computer setup](https://www.jeffgeerling.com/blog/2021/dont-fall-love-your-mac%E2%80%94automate-it)
