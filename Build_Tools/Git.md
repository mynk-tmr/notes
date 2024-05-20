**Links**: [Git tools](https://git-scm.com/book/en/v2/Git-Tools-Revision-Selection) , [Resolve Merge Conflicts](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/addressing-merge-conflicts/about-merge-conflicts) , [Best-practices](https://www.theodinproject.com/lessons/javascript-using-git-in-the-real-world#additional-resources)
Install & Configure

```bash
sudo apt-get install git

# --local => for current repo only
git config --global core.editor "code --wait"
git config --global -e

#paste 
  [user]
    name = Mayank
    email = github@noreply.com
  [core]
    editor = code --wait
    autocrlf = input #[true if windows]
  [init]
    defaultBranch = main

#connect to github
ssh-keygen -t rsa -b 4096 -C "xyz@yahoo.in" #all yes
# copy id_rsa.pub > github settings > add SSH key > paste
# test
ssh -T git@github.com
```

Most common…

```bash
git init /path/
git rm/mv  #rm/mv + track
git clean -fd #delete all untracked stuff
git rm -r --cached files #untrack files
```

Restore Files…

```bash
git checkout *sha* -- *files* #restore files from commit
git restore -p #interactive restoring files
git checkout-index files #restore from index
```

Compare …

```bash
git diff *one* *two* #compare file, branch, commits, etc.
git diff HEAD # wkdir , --staged for index
```

Actions on files/folders …

```bash
git diff --name-only HEAD~1 #files changed by recent commit
git ls-tree -r HEAD~2 #get sha & file/folders in it
git ls-files #list tracked files recursively
git status --ignored #see untracked files
```

Get details …

```bash
git config --get key
git show obj #details
git hash-object file #show sha-1 
git ls-files -s folder | git hash-object --stdin #show sha-1
git blame app.js #see latest changes + author
#-L 1,4 --> limit to lines
```


Git Log — browse history

```bash
git log [>> dev.log] #display
git shortlog [-n][--reverse] #only msgs [sort options]
git reflog     #includes changes to HEAD pointer
```

Flags for **git log**

```bash
--grep="steel" #commits having substring steel
-4   #display last 4 commits
--before="yesterday" --after="2023-04-15"
--author="John\\|Mary" # commits by John or Mary [\\ allows regex operators]
--follow <files>
--stat       #no. of insertions, deletions to each file in every commit
-S"hello" #commits which added hello to any file
-G"hello" #regex
--show-notes  #also display author notes
--no-merges #avoid merge-commits 
--merges  #show only merge-commits
```

Pretty Formats

```shell
$git log --pretty=format:"%cn committed %h %cr"

#%n : newline
#%Cred %Cgreen %Cblue %Creset : change colors
#%cn %ce %cl :  committer name, email and local-part
#%ch %cr : committed date (human style, relative )
#%H %h : commit hash (full, shortened)
```

Git stash — save current working directory

```bash
git stash [push -m "msg"]  #save
#other funcs
list, clear, apply, drop (rm) , pop (apply+rm)

git stash pop stash@{2}
```

Git tags

```bash
git tag --list v1.* #list matched
git tag -a `v1.0` HEAD #Creates light tag ; opens msg editor
git tag v1.2.2 -m `Release version 1.2.2` #good tag
```

Git Notes — Only 1 note is allowed per commit.

```bash
# default : HEAD 
git notes [add | append] <c> -m "Mynote" #add will overwrite
show <c> ; edit <c> ;remove <c>
 
git notes copy [-f] <From> <To> #use -f to overwrite pre-existing 
```

merge project A into project B

```bash
cd path/to/B
git remote add A /pathto/
git fetch A --tags
git merge --allow-unrelated-histories A/main
git remote remove A
```


---
Git has two main data structures - **object store & index,** stored in `.git` folder

**Git objects** represent a data structure and have a unique hash-id. Types

- **Blobs** (file structures)
- **Trees** ( directory structure)
    - **tree-ish** is anything that ultimately leads to a tree object. e.g. **branch, tag**
    - **commit-ish** (type of tree-ishes) e.g. `HEAD, sha-1 hash`
    - **combined** : tree-ish:path e.g. `main:assets/hello.js`

---
### Branch

- A `branch` is pointer to last commit made on branch. Internally, it's a **file** that contains sha-1 checksum of last commit
- new `branch` -> new copy
- `HEAD` is pointer to current local branch ; when it points to a specific commit -> detached

2 ways to integrate branches
- merge : combine end-points and point to it.
	- **fast-forward**: move main branch's tip forward onto feat tip
	- **2 parent**: new commit is created (merge commit)
- rebase : replay all commits in a particular branch over a different branch

```shell
cat .git/HEAD #ref: refs/heads/main
```

```shell
#USUAL FLOW

git switch production
git switch -c hotfix #new branch, now do changes
git switch - #go back to prod
git merge hotfix  #merge into current
git push production origin  #push branch
git branch -d hotfix  #delete
```

```shell
#logging branches
git log mybranch 
git log --all 

#list
git branch -v #last commit in each branch
git branch -vv # -v + which tracking
git branch --merged / --no-merged production #only these w.r.t a branch
git branch -a, -r # all / only remote
git log main..feat #show extra commits in feat
git diff HEAD..origin/feat  #after fetch

#rename
git branch -m old new 
git push -u origin NEW && git push origin -d OLD 

#fix merge conflicts, stage it
git mergetool 

# restoring branch
git checkout <sha>

git subtree split -b newb --prefix dist/  #create new branch with dist folder contents
```

```shell
#experimental commits
git checkout sha 
... do commits
git branch newfeat #stored here
```

### Remote 

USUAL WORKFLOW
```bash
#fork Opensource first
git clone forkedURL  
git remote add <real_repo_url> upstream
git switch -c feat
#...build stuff
git switch main && git pull upstream main
git switch feat && git merge main #to clean feat, solve conflicts
git push origin feat 
#...submit pull request on original page
```

```bash
git clone -b bname <url> -o ape #defaults main, origin
git remote #list repos
git remote remove yuki

#clone to folder
git subtree add --prefix temp <url> main --squash 
git subtree pull ....
git subtree push .... #no --squash

#branching, tracking
git fetch --prune ape  #get new changes
git fetch --all --tags #from all remotes
git checkout fetched_branch #creates it locally
git branch -u origin/fetched_branch  #remote tracking

#push, pull
git push -u --all --tags origin
git pull --rebase #use this when someone else rebases
git push -force-with-lease #safe force 
```

### Rewriting history

```shell
git rebase main #current on main's tip
git rebase --onto main server client #rebase client, excluding commonality with server
git switch main && git merge client
```

```bash
git reset [--soft] <commit> #go back, soft preserves wkdir+index
git reset HEAD@{n}  #git reflog
git commit --amend  #replace last commit with this one

git revert [--no-commit] <commit> #creates a new commit that undo changes of given commit
git revert --abort #stop reverting

git rebase -i <commit> # rolls back commits after <commit>
git rebase -i --root #first commit
  --committer-date-is-author-date 
```

##### Commiting Guide

```shell
feat(core): add "Blade3" in weapon.json #header -> type(scope): subject 

add new weapon #body -> motivation of change, contrast with original

BREAKING CHANGE: methods must implement CatchFatal() #footer -> closed issue or breaking change
```

- max 75 chars in a line ; only lowercase, no full-stop, present-tense
- types -> *build, ci, docs, feat, fix, perf, refactor, style, test, revert, chore*

**Revert**
```shell
revert: #header of reverted commit

This reverts commit #sha
```

