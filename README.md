##  ansible playbook for install rstudio server

### installation

- install debian-10 (buster) on the server as LXC container
- install r-base:
  - `apt-key adv --keyserver keyserver.ubuntu.com --recv-key '95C0FAF38DB3CCAD0C080A7BDC78B2DDEABC47B7'`
  - add `deb http://cloud.r-project.org/bin/linux/debian buster-cran40/` to /etc/apt/sources.list
  - `apt update && apt install -t buster-cran40 r-base`
- update `hosts` and `_default.yml`
- `ansible-playbook -i hosts playbook.yml`

### common dependencies

- sudo apt install nfs-common
- sudo apt-get install -y libxml2-dev libcurl4-openssl-dev libssl-dev
- sudo apt-get install libmysqlclient-dev
- sudo apt-get install libmariadb-dev
- sudo apt install curl
- sudo apt-get install libcurl4-openssl-dev r-base
- sudo apt install libssl-dev
- sudo apt-get install apt-transport-https ca-certificates gnupg
- echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt cloud-sdk main" | sudo tee -a /etc/apt/sources.list.d/google-cloud-sdk.list
- curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key --keyring /usr/share/keyrings/cloud.google.gpg add -
- sudo apt-get update && sudo apt-get install google-cloud-cli
- sudo apt install update-locale
- sudo apt install rsync

### setup authentication

- `cp /etc/pam.d/login /etc/pam.d/rstudio`
- `adduser rstudio` and setup password
- go to `IP:8787` to login with `rstudio`

### references

- https://cran.r-project.org/bin/linux/debian/#debian-bullseye
- https://linuxize.com/post/how-to-install-r-on-debian-10/
- https://docs.rstudio.com/ide/server-pro/authenticating_users/pam_authentication.html
- https://community.rstudio.com/t/rstudio-server-unauthorized-user/113071

## manual installation using ubuntu

Both Debian and Manjaro/Arch are not stable for using rstudio-server together with keras, it seems Ubuntu is the current best option.

### install nvidia drivers, CUDA and other dependencies for relevant pip pakcages

- sudo ubuntu-drivers autoinstall
- sudo apt install nvidia-cuda-toolkit nvidia-cuda-toolkit-gcc
- sudo apt install make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev

> NOTE: make sure disable `nouveau` modules in /etc/modprobe.d/blacklist.conf

### install rstudio

- sudo apt-get install r-base
- sudo apt-get install gdebi-core
- wget https://download2.rstudio.org/server/jammy/amd64/rstudio-server-2023.12.0-369-amd64.deb
- sudo gdebi rstudio-server-2023.12.0-369-amd64.deb
- sudo systemctl enable rstudio-server

### install keras in `rstudio`

```
install.packages("keras")
install.packages("reticulate")

library(reticulate)
library(tensorflow)
library(keras)

reticulate::install_python(version = "3.10")
keras::install_keras()

k <- backend()
tf$sysconfig$get_build_info()
```

