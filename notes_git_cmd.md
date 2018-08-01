After git clone , create a new branch so everyone works on own branch

To create a new branch

```cmd
git checkout -b <your branch>
```

Move to branch

```cmd
git checkout <some other branch>
```

After you have done bunch of stuffs on your branch:

```cmd
git add .
git commit -m 'msg'

// to make sure you are good
git status

// see how many commit you have done so far
git log
// combine first 4 (lets say 4) commits into 1
git rebase -i HEAD~4
// squash the last 3 commits by changing the prefixed into s
// save n exit
// set the ultimate commit msg
// save n exit

// git log to check if it is good

git push origin <your branch>
```

Pull stuff from other's branch

```cmd
git pull --rebase origin <other branch>
```

After someone made new branches , fetch(update) them to your local machine

```cmd
git fetch --all
```

To merge with other's work on local branch

```cmd
git add .
git commit -m 'some msg'
git git rebase -i HEAD~<n>

git pull --rebase origin <branch you want to merge>

// now resolve conflicts in vscode
// after everything resolved
git add .
git rebase --continue

// keep resolving conflicts until git says it is done
```



