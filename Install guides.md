## GIT

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

## NVM NODEJS

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