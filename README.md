# Preventing push to master or develop branch using pre-commit and pre-push git hooks with husky

If you want to prevent anybody from mistakenly commit or push to your master or develop branch. This is going to be one of your solutions.

## You should not use these scripts if...

- Your github repo is set as public, use built-in protected branch feature instead.
- You are using gitlab or bitbucket repo, use their built-in branch permission setting instead.

Note: preventing a push to a certain branch is recommended to be handled from server side.

## You may use these scripts if...

- You are using github.
- Your repository is set as private.
- You don't want to pay for just using protected branch feature.
- You don't want to distribute your pre-commit and pre-push script manually to everybody in your repository.
- You want to apply local-check before somebody push or commit to a certain branch.

## What is this all about?

These bash scripts are basically similar to the one that is located inside `.git/hooks/`. However we will tweak it and put it inside our repository and the scripts will be called by husky [https://github.com/typicode/husky](https://github.com/typicode/husky) (Husky can prevent bad git commit, git push and more 🐶 woof!) so it will be distributed to all members in your repository.

These scripts will be executed everytime there is a push or a commit to your repository.

## What will these scripts do?

This script will block anybody who tries to push to a certain branch (in my case I don't want anybody -including myself- to push directly to master and develop branch). They need to work in their own branch and then create a pull request.

This script will block anybody who tries to push to a branch that is different from their current active branch. For example you are in branch `fix/someissue` but then you mistakenly type `git push origin master`.

This script will block anybody who tries to commit to a certain branch (in my case master and develop branch).

This script will block anybody who tries to commit to a branch that has ascii character in it.

This script will block anybody who tries to commit to a branch that has upper case character in it.

## What to prepare and How to use? 

I will try to explain from the very begining. 

#### 1. Install Husky in your repo

`npm install husky --save-dev`

if you cannot run `npm`, you need to install nodejs and npm in your system first. Read this https://docs.npmjs.com/downloading-and-installing-node-js-and-npm

#### 2. Modify your package.json file

This file will be located in the root directory of your repository.

Open `package.json` file using vscode or sublime or atom or any text editor (vim / nano if you are using terminal) and add this several lines after dependencies:

```
"dependencies":{
    ...
},
"husky": {
    "hooks": {
        "pre-commit": "./commands/pre-commit",
        "pre-push": "./commands/pre-push $HUSKY_GIT_STDIN",
    }
}
```

You may combine the process with `lint-staged` or `commitlint` (optional). If you do, your `package.json` may look something like this:

```
"dependencies":{
    ...
},
"husky": {
    "hooks": {
        "pre-commit": "./commands/pre-commit && lint-staged",
        "pre-push": "./commands/pre-push $HUSKY_GIT_STDIN",
        "commit-msg": "commitlint -E HUSKY_GIT_PARAMS"
    }
},
"lint-staged": {
    ...
},
```

Read here for full instructions of how to prepare linting to your repository:\
(Laravel + PHP CS Fixer + Prettier + ESLint + airbnb-base + Husky + lint-staged + commitlint + pre-commit hook + pre-push hook)\
https://gist.github.com/christchan/4459e9095dac0e21aae996dde2fb788b


#### 3. Download commands folder from my github and paste it in the root of your repository folder

```
...
node_modules/
commands/
|- pre-commit
|- pre-push
package.json
...
```

#### 4. Modify the scripts and adjust it to your own rules.

These default scripts are suitable for my needs, however it may need adjustment for others, feel free to adjust and let me know if there is a better way to do.

If you think this article helps you, You can buy me a coffee ☕ https://www.buymeacoffee.com/christchan

Enjoy!\
www.talenavi.com
