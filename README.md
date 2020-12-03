# eitan-tikal-nodejs
wizeup
This code will bring nodejs http server that responde "HelloWorld!" to any http request

The repository is based on (https://github.com/fhinkel/nodejs-hello-world) repoistory.
three main diffrences are:
1. abiliy to run the nodejs on docker container
2. new uri added: /test - to test is web site is alive
3. jenkins ci support

## Installation

in order to run the nodejs server u need to perform the following steps:
1. clone repository :
```
git clone https://github.com/eitanbenjam/tikal_eitan_exam.git
```
2. after repository cloned to your filesystem, we need to build the docker image on out docker
```
   cd tikal_eitan_exam
   image_name="nodejs-hello-world"
   docker build --tag ${image_name} .
```
3. check that docker image was created by running the command:
```
    docker images|grep ${image_name}
```
   output should be:
```
   nodejs-hello-world             latest              ae20716bca86        11 seconds ago      921MB
```
4. after docker image was build we can run it with the following commad:
```
    listen_port=8888 # this is the port that u need to connect in order to connect to the container
    docker run -d -p ${listen_port}:80 ${image_name}:latest
```
5. to test the container we need need to http request both urls:
```
curl http://localhost:${listen_port}/ # expected result should be: Hello World!
curl http://localhost:${listen_port}/test # expected result should be: OK
```
    
