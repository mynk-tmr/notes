##### Github
- Caution
    1. never `git amend/reset` commits pushed to remote.
    2. never `git rebase` a branch, others are working on
    4. never `git push --force` 

##### Javascript
- breakpoints over console.log 
- log only *stringified* object in complex apps
- comment on complex *regex*
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

## SOLID Principles

**Single Responsibility** => a method/class/component [entity] should have a single reason to change i.e. what it's responsible for. Common violation -> tightly coupled objects

**Open/Closed** => an entity should be open for extension, but closed for modification.

**Liskov Substitution** => code should work even if objects of a superclass are substituted with objects of its subclass. Failing LSP usually mean bad inheritance ; common indicators to know violations - using instance of checks ; empty methods ; throwing errors that superclass can't

**Interface Segregation** => an entity shouldn’t be forced to implement or depend on methods it never uses. Violations -> very much like LSP ones. Solved via `object composition`

**Dependency Inversion** => high-level modules (complex logic) should not import anything from low-level modules  and communicate via `abstract` interfaces (a wrapper around APIs).