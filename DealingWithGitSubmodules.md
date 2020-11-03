# Dealing with GIT Submodules

## Contents

## Overview
This document has the purposes to offer useful commands, tips and tricks and other useful information in dealing with GIT Submodules within a Node.js environment. Do keep in mind that the advices here can be used for any other sort of project (E.g. Python, Java).
Keep in mind that in recent times, a lot of effort was put into making GIT work flawlessly across Linux, MacOS, Microsoft Windows. So everything in this tutorial should work across all major operating systems.

## Cases

#### Clone the repo

First of all you need to clone your desired repository by executing the following command: 

```text
git clone --recursive {your-repository-clone-link}
```

So your command will look something like this: 

```text
git clone --recursive git@github.com:robertdumitrescu/docs.git
```

After clonning the repo go inside the repo by using the following command: 

```text
cd your-repository
```

#### Install Git submodules (optional) (if you missed the --recursive in the previous step)

If you missed the ```--recursive``` part in the git clone, or the project was already cloned but didn't had any submodules in the past, you can fix that by running the following 2 commands:

```text
git submodule init
git submodule update
```

This will initialize the submodules according to your ```.gitmodules``` file.

#### Install specific Git submodules (optional)

If you are in the situation where all the submodules are cloned and installed except 1 or 2 you can clone the missing one by running a command specifically for them:

```text
git submodule update --init {name_of_app}
```

So your command will looks something like: 
```text
git submodule update --init YOUR_APP
```

#### Install node modules for all the Git submodules

For that, use the following command:

```text
git submodule foreach npm install
```

#### Adding a new project to the stack

##### To the root folder

In order to do that we will run the following command which will add another git submodule to the working stack:
```
git submodule add {your-repository-clone-link}
```

So your command will look something like: 
 
 ```
 git submodule add git@github.com:robertdumitrescu/docs.git
```

##### To a specific sub-folder

In order to do that we will run the following command which will add another git submodule to the working stack:
```
git submodule add --force {your-repository-clone-link} {your-subfolder-location}
```

So your command will look something like: 
 
 ```
git submodule add --force git@github.com:robertdumitrescu/docs.git src/docs
```

### Updating git submodules

First of all, you need to go in each directory and commit everything that you worked on, pull, and push all the changes.

Then, when you are in the main project that is referencing the GIT submodules you will see the following for the projects that suffered changes by running the following:

```
git submodule foreach git status
```

That will produce and output like:
```text
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   BEAMS (new commits)
        modified:   BEAPPS (new commits)
        modified:   BECONMS (new commits)
        modified:   FECONMS (new commits)
        modified:   theswissguide (new commits)

```
After that, you can run the following command for the repository to pull the last reference (the last commit pushed in each of the submodules earlier):

```
git submodule update
```

#### Switching to master branch on all projects

```
git submodule foreach git checkout master
```

#### Showing git status of all projects

```
git submodule foreach git status
```

#### Showing current branch on all projects

```
git submodule foreach git branch
```

### Purging (Deleting) node_modules in all GIT submodules

In order to delete all the node_modules use the following command linked to each git submodules:

```
git submodule foreach rm -rf node_modules
```

After all the apps are free from any old node_modules, you can re-install them using the following command:

```
git submodule foreach npm install
```

## Macros

### Update all repositories

```
git submodule foreach git status
git submodule foreach git checkout -- .
git submodule foreach git checkout master
git submodule foreach git pull
git submodule foreach rm -rf node_modules
git submodule foreach npm install
```

### Push commits on all repositories at the same time

This is particularly useful for times when you are working on a new feature that is affecting multiple repositories (submodules) and it makes sense to commit on all of them at the same time with the same commit message.

```
git submodule foreach git status
git submodule foreach git add -A
git submodule foreach 'git commit -am "{commit_message}" || :'
git submodule foreach git pull
git submodule foreach git push
```

## Troubleshooting

#### JetBrains products integrations
If you run the whole project in your IDE and you want the IDE to highlight your changes in each git submodules, you need to install the following IDE extensions:

- GitStatus
- Git Tool Box

Also for the files to not get resetted when you do external changes you need to go and add more different VCS roots.

- Go to Settings > Version Control
- There you will see that just the main repository is added as root source but the other submodules are detected as well. 
- You need to add all the other submodules as root sources in order for the IDE to track everything properly.

#### (MS Windows only) Why GIT repositories are not recognized in mounts?

In case you have a submodule GIT project mounted inside a Linux machine (Ubuntu, Fedora) and even Apple Mac, GIT submodule metadata is syncronized in the child project as a symlink. Because git doesn't support any sort of smart symlinking, you need to make the Linux distribution to poll the files from the hard files in host machine, to the guest machine and do a hard copy on the guest hard drive. 

This pose some serious problems like:
- .git files getting out of sync
- both Windows and Linux working more then is needed
- Slow performance when accessing large projects refs 

Windows is trying to implement a compatible behaviour with Linux. Watch this issue:

- [Symlinks on the mounted Windows directories are not compatible with native.](https://github.com/Microsoft/WSL/issues/353)

Due to the fact that Hyper-V uses samba for file virtualization. The behaviour need to be propagated in Samba first and this is planned to be solved in 2020.
