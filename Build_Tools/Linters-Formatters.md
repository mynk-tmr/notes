package : a directory with js modules or libraries. Must have `package.json` file.

package manager : automates installing, upgrading, configuring, removing packages; help developers manage dependencies.

module bundler : to create a single static file from multiple files. It builds a dependency graph from `entry` point(s) [starting file] and then combine them into a single `output` file.

transpiler : converts code from other languages to ones browser understands

task runner: To automate different parts of the build process
## ESLint

install plugin > ctrl+shift+P > user settings > paste
```json
  {
    "editor.codeActionsOnSave": {
      "source.fixAll": true
    },
    "eslint.validate": ["javascript"]
  }
```
npm i -D eslint && npm init @eslint/config

More learning :- https://eslint.org/docs/latest/use/core-concepts