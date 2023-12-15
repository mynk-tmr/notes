**Links**: [Git tools](https://git-scm.com/book/en/v2/Git-Tools-Revision-Selection) , [Resolve Merge Conflicts](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/addressing-merge-conflicts/about-merge-conflicts)

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

#.gitignore /log (in root), log/ (anywhere)

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
git hash-object mix.css #show sha-1 of file
git ls-files -s src | git hash-object --stdin #show sha-1 of folder src 
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

```c
$git log --pretty=format:"%cn committed %h %cr"
	Sainki committed 7477ad4 2 hours ago
	Harsh committed 636s29 3 hours ago

%n : newline
%Cred %Cgreen %Cblue %Creset : change colors
%cn %ce %cl :  committer name, email and local-part
%ch %cr : committed date (human style, relative )
%H %h : commit hash (full, shortened)
```


Git aliases — create custom git commands

```bash
open .config file and paste
[alias] 
newinfo = "!f() { VAR=$1; OLD=$2; NEW=$3; shift 3; git filter-branch --force --env-filter \\"if [[ \\\\\\"$`echo $VAR`\\\\\\" = '$OLD' ]]; then export $VAR='$NEW'; fi\\" $@; }; f"

# Change author info retroactively
git newinfo GIT_AUTHOR_NAME "oldname" "newname"
git newinfo GIT_AUTHOR_EMAIL "oldmail" "newmail"
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
git tag --list [*pattern*] #list all/matching tags in repo 
git tag -a `v1.0` HEAD #Creates light tag ; opens msg editor
git tag v1.2.2 -m `Release version 1.2.2` #create good tag
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
---

Git is a **distributed VCS** and Github is a **hosting service** for Git repositories.

Git features

- version control : records changes in a project in a special database called **repository**.
- Git has two main data structures - **object store & index,** stored in `.git` folder in each project
- _distributed_ - each user has their own repo, which they use to update a remote repo.
    - in local vcs, all changes are on local storage
    - in centralised vcs, only one global repo exists for for all users. Risky because it has a single point of failure

**Git objects** represent a data structure and have a unique hash-id. Types

- **Blobs** (file structures)
- **Trees** ( directory structure)
    - **tree-ish** is anything that ultimately leads to a tree object. e.g. **branch, tag**
    - **commit-ish** (type of tree-ishes) e.g. `HEAD, sha-1 hash`
    - **combined** : tree-ish:path e.g. `main:assets/hello.js`


### Branch

- A `branch` is pointer to last commit made on branch. Internally, it's a **file** that contains sha-1 checksum of commit
- new `branch` -> new copy
- `HEAD` is pointer to current local branch ; when it points to a specific commit -> detached

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
git push -d/-D hotfix  #delete
```

```shell
#logging branches
git log mybranch 
git log --all 

#list
git branch -v #last commit in each branch
git branch -vv # -v + which tracking
git branch --merged / --no-merged <hotfix> #only these w.r.t a branch
git branch -a, -r # all / only remote
git log main..feat #show extra commits in feat
git diff HEAD..origin/feat  #after fetch

#rename
git branch -m old new 
git push -u origin new  #old still on remote
git push origin -d old #delete

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

```bash
git clone -b bname <url> -o ape #defaults master, origin
git remote #list repos
git remote add <url> yuki
git remote remove yuki

#clone to folder
git subtree add --prefix temp <url> main --squash 
git subtree pull ....
git subtree push .... #no --squash

#branching, tracking
git fetch [--prune] ape  #get new changes
git fetch --all --tags #from all remotes
git checkout feat #create feat locally, track it
git branch -u ape/feat  #track feat from current branch
git checkout -b cust ape/feat #for custom name

#push, pull
git push -u --all boogie && git push --tags
git pull --rebase #use this when someone else rebases
git push -force-with-lease #safe force 
```

### Rewriting history

2 ways to integrate branches
- merge : combine end-points and point to it. e.g. fast-forward or 2 parent
- rebase : replay all commits on 1 branch on a different branch

```shell
git rebase main #current on main's tip
git rebase main feat #given
git rebase --onto main server client #rebase client, excluding commonality with server

#finally
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

#flags
  --committer-date-is-author-date 
```