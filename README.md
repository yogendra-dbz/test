Windows
-------
*Modify to suit your preferred configuration, this configuration tested on Window 10 & Window Subsystem for Linux (WSL) *

1. Setup the Ubuntu 16.04 WSL  https://docs.microsoft.com/en-us/windows/wsl/install-win10 (On Windows)

2. VirtualBox (https://www.virtualbox.org/wiki/Downloads) (on Windows)

```
Download & install the Virtualbox for window  https://download.virtualbox.org/virtualbox/6.0.12/VirtualBox-6.0.12-133076-Win.exe

Download & install the VirtualBox 6.0.12 Oracle VM VirtualBox Extension Pack https://download.virtualbox.org/virtualbox/6.0.12/Oracle_VM_VirtualBox_Extension_Pack-6.0.12.vbox-extpack

```

3. Login to WSL and execute all steps 


  - Open the window CMD terminal and type 
  
  `C:\> bash`
  
  
  - Setup bash to load additonal scripts
```
  cat <<EOF >> ~/.bashrc

  ########## Bash scripts ##########
  for i in "$HOME"/.bash/*.sh; do
      [[ -r "$i" ]] && source "$i"
  done
  EOF
```
 
4. git, bash, build-essential

   `apt install git bash build-essential`

5. Git Flow (https://github.com/petervanderdoes/gitflow-avh)

  5a. Use the Distro version if it is newer than 1.11.0

   `apt install git-flow`


  5b. Otherwise use the version directly from the project
  - Assume that you have D:/ drive in your window, then it follows /mnt/d/ path in WSL. otherwise use drive letter appropriately in WSL 
  - Check out git-flow

```
mkdir -p /mnt/d/projects
export PROJECTS=/mnt/d/projects
cd "${PROJECTS}"
git clone https://github.com/petervanderdoes/gitflow-avh.git
```

  - Make bash add gitflow to path
```
mkdir -p ~/.bash
cat <<EOF >> ~/.bash/gitflow.sh
PATH="${PATH}:${PROJECTS}/gitflow-avh"
EOF
```

6. Ruby Version Manager

  There are a lot of options for this

6a. RVM

  See the OSX instructions

6b. rbenv (https://github.com/rbenv/rbenv), direnv (https://direnv.net/)

see step 7 in this section and make sure your
`bundle install` includes  `--binstubs --path .bundle`.

Also make sure you have direnv setup to add the project bin directory to your path to
ensure you are running the knife, kitchen, etc commands from the right gem versions.

Rbenv, see instructions on website, but tl;dr;
```
git clone https://github.com/rbenv/rbenv.git ~/.rbenv

cat <<EOF >> ~/.bash/rbenv.sh
########## RBENV Setup ##########
if [[ -d "$HOME/.rbenv/bin" ]]; then
    path_add -e "$HOME/.rbenv/bin"
    eval "$(rbenv init -)"
fi
EOF

source ~/.bash/rbenv.sh
mkdir -p "$(rbenv root)"/plugins
git clone https://github.com/rbenv/ruby-build.git  "$(rbenv root)"/plugins/ruby-build
git clone git://github.com/tpope/rbenv-aliases.git "$(rbenv root)/plugins/rbenv-aliases"
rbenv alias --auto
```

direnv
```
wget https://github.com/direnv/direnv/releases/download/v2.19.1/direnv.linux-amd64  -O ~/bin/direnv

cat <<EOF >> ~/.bash/direnv.sh
eval "$(direnv hook bash)"
EOF
```

6c. asdf (https://github.com/asdf-vm/asdf), direnv (https://direnv.net/)

This is very similar to rbenv/direnv. See those instructions and modify as appropriate.


6d. Vagrant (https://www.vagrantup.com/downloads.html)

  * Download & install the vagrant debian package version 2.2.5
  * Update the home directory environment variable, for eg. you have D:/ drive on window and your project folder location is D:/ (Required for WSL + vagrant to work)
  * Note: 
    - your ~/.bashrc location would remain your origin $HOME path and can be accessible using /home/<user>/.bashrc  instead of ~/.bashrc
    - $HOME variable would start refering to your project location
    - Environment variable for enable vagrant to work with window & Virtualbox (https://www.vagrantup.com/docs/other/wsl.html).


```  
cat <<EOF >> ~/.bashrc
export PATH="$PATH:/mnt/c/Program Files/Oracle/VirtualBox"
export VAGRANT_WSL_ENABLE_WINDOWS_ACCESS="1"
export VAGRANT_IGNORE_WINRM_PLUGIN="1"
export VAGRANT_DISABLE_VBOXSYMLINKCREATE=1
export VAGRANT_HOME="${PROJECTS}/.vagrant.d"
export VAGRANT_WSL_WINDOWS_ACCESS_USER_HOME_PATH="${PROJECTS}"
EOF
  
source ~/.bashrc

```


   - Windows Hyper-V that must be disabled in Control Panel -> Program And Features -> Turn Windows Features on or off
   - Add additional kitchen properties in your .kitchen.yml or .kitchen.local.yml


    ```
    ---
    driver:
      kitchen_cache_directory: kitchen_cache
      ssh:
        insert_key: false
    ```

