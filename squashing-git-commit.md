---
title: Squashing git commit
subtitle: 'Hide your sins by combining commits'
description: Git allows rewriting history to have a clean version. Rebase command that enables you to clean up the frequents commits by offering to squash.
keywords: git, git squash, git clean history, git rewrite history, rebase, squash commit
datePublished: '2018-06-17'
lastModified: '2020-02-02'
tags: 'git, tutorial'
---

Git is a leading version control software (VCS). There may be a time in your experience when you have to commit very often with a small change.

Typically, Feedback Loop involving work leads you to commit frequently.

- To fix a problem, in a message-driven application, the impact of a fix is hard to foresee in downstream systems. The well-known technique partial fix and verify.
- You are experimenting with a configuration file driven system like CI ( Jenkins, Travis). You have to frequent commits to check or learn the effect of the configuration changes.

Whatever the reason, if you look at the history of the git repository; it may frown you or anyone.

![bad commit history](https://miro.medium.com/max/2732/1*jX0ARIpb2YAp0vZfIo_PhA.png)

<figcaption>Last 3 commits are related; All can be combined to one commit</figcaption>

Don’t worry; there is a three-steps process that helps you to hide your sins.

## Step 1: Freeze branch

Forbid anyone to push any code to the desired branch.

## Step 2: Rebase

Rebase command let you move commits up or squash.

```sh
git rebase -i HEAD~<no-of-commits>

# Example : To squash last three commits
git rebase -i HEAD~3
```

It opens an editing window and lists the selected commits in chronological order _(the first in time tops first)_.

![rebase commit window](https://miro.medium.com/max/2732/1*lsrZWMPflYbATUqFvRDvrw.png)

<figcaption>commits in chronological order</figcaption>

There are multiple commands available, one of them is squash which says:

```
# s, squash = use commit, but meld into previous commit
```

Type `squash` by replacing pick to make commit combined with the last commit.

In this case, I am going to combine last two commits to first one. So I typed `squash` for last two commits. _Isn't simple ?_

![](https://cdn-images-1.medium.com/max/2400/1*TtZNXAnD73AbE7ULCMTKSA.png)

<figcaption>bb5e3d1 ← bb5e3d1 + d1c8507 + 6d7c3cc</figcaption>

It is can be imagined as

_Mathematically_

```
bb5e3d1 ← bb5e3d1 + d1c8507 + 6d7c3c
```

_Visually_
![](https://cdn-images-1.medium.com/max/2400/1*UnFBSTQ31jzg0ryyHlvMHQ.png)

Once you saved commands, Git allows you to change the message of the newly combined commit.

![](https://cdn-images-1.medium.com/max/2400/1*s19RhFyIKJLp0hQmIekraw.png)

<figcaption>edit message</figcaption>

![](https://cdn-images-1.medium.com/max/2400/1*T64kb-Pax5rXiih1DDmVrw.png)

<figcaption>difference between history</figcaption>

There may be chances that you may come across conflicts. In that case, resolve conflicts as your usual process. Once done, the following command will put you back in the race.

```sh
git rebase --continue
```

For some reasons, you don’t want to continue. Don’t be afraid! Git forbids you in that case too.

```sh
git rebase --abort
```

## Step 3: Force Push

Force Push is crucial to cancel any remote update.

```sh
git push <remote-server> +<branch>  #notice + symbol
# Example
git push origin +master
```

---

You can try or experiment by forking the following repository on Github
https://github.com/theBeacon/sqaush-gitty-demo
Checkout a new branch from `feature/terrible-branch`, to play with squashing.

---

Git allows you to rewrite history. Squashing is one example.

Some may argue to use `amend` commit option, which allows rewriting the last commit. Remember, `amend` only works if the last commit is not pushed yet.

Some may also argue that it is tampering with history. Remember, It is about cleaning history.
