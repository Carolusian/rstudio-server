# ansible playbook for install rstudio server

## installation

- install debian-10 (buster) on the server as LXC container
- install r-base:
  - `apt-key adv --keyserver keyserver.ubuntu.com --recv-key '95C0FAF38DB3CCAD0C080A7BDC78B2DDEABC47B7'`
  - add `deb http://cloud.r-project.org/bin/linux/debian buster-cran40/` to /etc/apt/sources.list
  - `apt update && apt install -t buster-cran40 r-base`
- update `hosts` and `_default.yml`
- `ansible-playbook -i hosts playbook.yml`

## setup authentication

- `cp /etc/pam.d/login /etc/pam.d/rstudio`
- `adduser rstudio` and setup password
- go to `IP:8787` to login with `rstudio`

## references

- https://cran.r-project.org/bin/linux/debian/#debian-bullseye
- https://linuxize.com/post/how-to-install-r-on-debian-10/
- https://docs.rstudio.com/ide/server-pro/authenticating_users/pam_authentication.html
- https://community.rstudio.com/t/rstudio-server-unauthorized-user/113071
