
##### Github
- Never *hardcode* 'API key' in code, bad bots can see them
- Caution
    1. never `git amend/reset` commits pushed to remote.
    2. never `git rebase` a branch, others are working on
    4. never `git push --force` 

##### Javascript
- breakpoints over console.log 
- log only *stringified* object in complex apps
- comment only purpose ; not what's happening
- comment on complex *regex*
- follow TDD --> write tests for code, before actual code.
- avoid callback hell
	- avoid deep nesting & anon-funcs
	- define functions at *top*-level in code
	- handle errors in *all* callbacks
	- create reusable functions and place them in a *module*

##### Planning a website
- find out common sections on every page
- their rough diagram
- note down **uncommon** content among webpages
	- group **related** ones using card sorting
- create a **sitemap** from these groups

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


##### Monorepos (tbd)