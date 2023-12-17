# NPM

**Install nvm & node js**

```bash
sudo apt install curl  
sudo apt update && sudo apt upgrade
curl -o- <https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh> | bash

export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \\\\. "$NVM_DIR/nvm.sh"

#install node
nvm install --lts
nvm use --lts
```

## Common npm commands

```bash
npm -v
npm cmd -h #help
npm docs lodash #open doc website
npm i npm@latest -g #upgrade
npx pkg_cmd #run, skip npx in auto-scripts
npm run build #run script

npm init -y #auto init
npm init esm -y
npm get init-license #default value
npm set init-license 'MIT' #default value
npm config edit #open editor
npm config fix #repair config

npm i / rm lodash momo #install multiple ; can be .tgz, git url
npm ci #clean install project using pkglock

#pkg version
npm i -E pkg@2.1.2 #specific
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

## NPM Scripts

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

## package.json

`package.json` contains metadata about the project and its  dependencies. It helps npm to handle project

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
    "pem": "../foo/bar"
  },
  "peerDependencies": { //your pkg requires
	  "tea": "2.x"
  }  
}
```


---


Rest (from code splitting) => [https://webpack.js.org/guides/](https://webpack.js.org/guides/)

babel loader

```jsx
//install
npm i -D babel-loader @babel/core @babel/preset-env 
npm i -D @babel/preset-typescript 

//set .babelrc
{
  "presets": [
    [
      "@babel/preset-env",
      {
        "targets": { "browsers": [ "> 1%"]},
        "useBuiltIns": "entry",
        "corejs": "3.0"
      }
    ],
    [
      "@babel/preset-typescript"
    ]
  ]
}
```

---
