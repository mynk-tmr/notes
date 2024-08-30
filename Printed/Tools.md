### Terms
- **package** : a directory with js modules or libraries. Must have `package.json` file. 
- **package manager** : automates managing packages & dependencies. **npm** is default manager of nodejs.
- **module bundler** : creates a single static file from multiple files. It builds a dependency graph from `entry` point(s) and then combine them into a single `output` file.
- **transpiler** : converts code from other languages to ones browser understands. e.g. Babel
- **task runner**: To automate different parts of the build process like testing, linting, compiling etc. Commonly **npm scripts** are used.
- **version control system** :  software tools that help software teams manage changes to source code over time and revert back when required.
- **Git** : a distributed version control tool. It records changes in a project as **snapshots** in a database called *repository*
- **Test runners** : tools that execute a pre-defined test script and auto-generate results. e.g. Jest
- **Webpack**: a module bundler & compiler for JavaScript and other front end assets.


## GIT

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

## Common npm commands

```bash
npm docs lodash #open doc website
npm i npm@latest -g #upgrade

npm get init-license #default value
npm set init-license 'MIT' #default value
npm config edit #open editor
npm config fix #repair config

npm i / rm lodash momo #install multiple ; can be .tgz, git url
npm ci #clean install project using pkglock

#pkg version
npm i -E pkg@2.1.2 #exact
npm i -S pkg@1.2 #~1.2.latest
npm i -S pkg@4 #^4.L.L
npm i -P pkg #install as dependecy
npm i -D pkg #install as dev-dependency

# other flags -g (global), -O (optional dep.)

npm ls --depth 0 #list installed packages
npm ls # + all dependency tree

npm outdated #list outdated pkgs
npm up [pkg] #update all or given
npm up --dev #only dev
npm prune #remove unused pkgs

npm view pkg #show info
```

## package.json

`package.json` contains metadata about the project and its dependencies. It helps npm to handle project

`package-lock.json` records exact version of each installed package & dependencies -- to build identical dependency tree in every dEV environment. PUSH this.

**Version range**
- `~1.0.4` => stick to minor (++patches)
- `^1.1.0` => stick to major (_default ; ++features_)
- `2.3.2` => exactly this, no auto-upgrade

Properties of package.json
```json
"name" //url safe project name
"version" //1.0.2
"description" 
"main": "index.js"  //entry point of app
"browser" //use instead of "main" for FE
"author": {} //name, email, url
"contributors": [] //authors
"bugs": {}  //url (of issues), email
"homepage"  //url 
"repository"  //type (git) , url
"config": {}  //like port
```

other dependencies
```js
{
  "dependencies": {
    "foo": "1.0.0 - 2.9",
    "bar": "<2.1.2",
    "asd": "http://asdf.com/asdf.tar.gz",
    "wsad": "../foo/bar"
  },
  "peerDependencies": { //your pkg requires
	  "tea": "2.x"
  }  
}
```

#### NPM Scripts

written in `scripts` object. key is *life-cycle* event, value is cmd to run. Always runs from *root* dir

Best practices
- only for what nodejs can't do
- dont' use `sudo`
- place them in `/scripts` folder

```js
//define pre & post scripts
premyscript, myscript, postmyscript

//run scripts of dependencies
npm explore pkg -- npm run cmd

//useful inbuilt
prestart, start, poststart, stop, restart, build

//test
pretest test postest

//run multiple
"all": "npm-run-all --serial/--parallel start test"

//use files
"install": "scripts/install.js",
```

## Emmet

| **CSS**                                                                                                                                                                                                                                                                                       |                                                                                                                                                                                                                               |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `-prop` - suggest vendors<br>`bg+` - shorthand autofill<br>`bxsh` : box-shadow autofill<br>`tac` - text-align:center;<br>`pos:s` - position: static;<br>`bdtc:t` - border-top-color: transparent;<br>`prop:no` - prop: none;<br>`@f+` - @font-face { src...}<br>`anim-` - shorthand animation | `m20` : px<br>`m20p` : 20%<br>`m20p-40` : 20% 40px<br>`m0-a` : 0 auto;<br>`fz2e` : font-size: 2em;<br>`fz.8r`: 0.8rem;<br>`cr` : color: rgb(0, 0, 0) [_tabs_ to jump edit ]<br>`animdur:30ms`<br>`p2r+fz16` : multiple styles |
| **HTML**                                                                                                                                                                                                                                                                                      |                                                                                                                                                                                                                               |
| Number<br>`$` replaced by no.  \|  `$@-` reverse no.  <br>`$$$` 001 002...     \|  `$@5`  start from 5<br><br>`c` : comment<br>`select+` `pic+` : add + to autofill required children                                                                                                         | ^ climb up<br>text {hello $}                                                                                                                                                                                                  |
