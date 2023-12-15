
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
git rm -r --cached *****files***** #untrack files
**git branch -a, -r, -d, -D, -m #list, delete, rename
```

Cloning

```bash
git clone [-b *bname*] *url  #can be local file path*
git subtree add --prefix temp *url* main --squash #clone into temp folder
#replace add with pull; push {no squash} for sync
```

Restore Files…

```bash
git checkout *sha* -- *files* #restore files from **commit**
git restore -p #interactive restoring files
git checkout-index ********************files #restore from **********index***********
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

git subtree split -b newb --prefix dist/  #create new branch with dist folder contents
```

Get details …

```bash
git config --get *key*
git show ***obj*** #details
git hash-object mix.css #show sha-1 of file
git ls-files -s src | git hash-object --stdin #show sha-1 of folder src 
git blame app.js *****#see latest changes + author
#***** **-L 1,4** --> limit to lines
```

Working with Github

```bash
git remote #list all remote repos
git remote add <name> <url> #link remote repo and give it name
git remote remove name
git push -u --all *name* #push all branches, create tracking references for branches
git push [-d] orgin/feat **#to specific
git fetch [--prune] origin/feat  #get changes
git diff HEAD..origin/feat  #see changes

#tags are explicitly pushed/fetched ; no pull
git push origin --tags

git pull [--rebase] #fetch + merge ; fix merge conflicts by prefixing alien commits
```

Git Log — browse history

```bash
git log [>> dev.log] #display
git log main..feat #show commits in feat, not in main
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

Changing commit history (!!! **Changes both index and working directory**)

```bash
git reset [--soft] <commit> #go back, soft preserves wkdir+index
git reset HEAD@{n}  #git reflog
git commit --amend  #replace last commit with this one

git revert [--no-commit] <commit> #creates a new commit that undo changes of given commit
git revert --abort #stop reverting

git rebase main #make current branch HEAD and apply its commits on tip of main
# Used to create Linear commit history

git rebase -i --committer-date-is-author-date <commit> 
# rolls back commits after <commit_id> and squash multiple commits into one
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
