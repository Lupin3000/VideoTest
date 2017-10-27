# VideoTest

This repository includes all files for the [tutorial](http://softwaretester.info/create-a-simpl…st-environment/ ‎E) on [softwaretester.info](https://softwaretester.info).

## Create project

```shell
# create project
$ mkdir -p ~/Projects/ && cd ~/Projects/

# clone all files from repository
$ git clone https://github.com/Lupin3000/VideoTest.git

# change directory
$ cd ~/Projects/VideoTest
```

## Run project

```shell
# modify Vagrantfile (box name)
$ vim Vagrantfile

# start new environment
$ vagrant up --provision-with install,prepare,start

# open in browser
$ open -a Safari http://localhost:8080/
```
