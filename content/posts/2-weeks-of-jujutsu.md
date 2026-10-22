+++
date = '2026-10-21T22:47:16-04:00'
title = '2 Weeks of Jujutsu'
description = 'Why I like using `jj` for version control.'
+++

No, I am not learning martial arts[^1].

---

A couple of weeks ago a coworker brought up this [blog](https://ersc.io/blog/what-comes-after-git) for East River Source Control (ERSC). They mentioned that ERSC recently hired the creator of [Jujutsu](https://github.com/jj-vcs/jj) to become their CTO.

Jujutsu (or `jj`) is a Git-compatible version control tool. I had heard of it but never tried it out. This time around, I read a couple of tutorials, listened to a few talks, and have been giving it a go.

This post isn't a tutorial[^2], but a brief explanation of some features that make `jj` attractive and how my workflow has improved with it.

### Why Jujutsu?

What kind of a name is Jujutsu anyway?! Well the origin behind the name is that `jj` is easy to type and once the binary was called that they had to come up with a name to match it. What about something like [Jon Jay](https://www.baseball-reference.com/players/j/jayjo02.shtml) (former St. Louis Cardinals OF)? Or maybe calling it `jjj` after J. Jonah Jameson.

### Improvements

This is not an extensive list of improvements (it's only been 2 weeks!) and I'm sure there are more effective ways to achieve the workflows listed below in git, but jj is really nice to use and it's made these workflows better for me.

#### Context Switching

The first QOL improvement I noticed after switching to `jj` was how easily I could interrupt working on `foo` in my repo with working on `bar`. With git this would require stashing my working changes or creating some WIP commit but since `jj` automatically takes a snapshot of your working tree it exists as a working commit above `foo`.

##### With Git
```
git commit -a -m "WIP: working on foo"
git switch main 
git switch -c bar
```

This isn't *necessarily* complicated but representing my working state as a sort of freeze feels off.

##### With jj
```
jj new main
jj bookmark create bar
```

This creates a new change with main as its parent commit *separate* from foo and attaches a bookmark (`jj`'s equivalent of a branch) to that change.

#### Reviewing PRs

I'm increasingly finding myself pulling down PR branches locally to use `claude` or `codex` to explain some changes.

##### With Git
```
git pull
git checkout pr-branch
```

Once again this doesn't feel very complex but it has the side-effect of leaving behind a bunch of local branches.

##### With jj
```
jj git fetch
jj new pr-branch@origin
```

With `jj new`, we create a new empty change that I can mess around on **on top** of the latest commit of the branch that I am reviewing. When I'm done reviewing a simple `jj new main` or `jj new my_branch` gets me right back to a clean state with nothing left over.

#### Rebasing

I still remember the day I learned about `git rebase -i` when I was an intern (thanks William). I felt like I had all of the power in the world. Well rebases can get annoying, especially when there are conflicts. In `jj` conflicts are **ok**, you'll still have to deal with them, but they're not a blocking matter. This means rebases always succeed and when rebasing with `jj` you aren't just rebasing a commit but its entire stack! This is extremely helpful when working on stacked branches which are all the rage. 

### Why does this matter? Is anyone still manually committing? 

AI tools can commit, rebase, organize my commits, and resolve my conflicts. I'd rather not depend on Claude to effectively read my repo's history, but maybe I'm spending my time learning the wrong things...

[^1]: I am not the first person to make this joke and I certainly won't be the last.
[^2]: Steve Klabnik's [tutorial](https://steveklabnik.github.io/jujutsu-tutorial/) is mentioned around everywhere I read and is the one I used to learn more about `jj`.
