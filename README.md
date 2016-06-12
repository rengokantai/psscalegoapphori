#### psscalegoapphori
install docker:
```
apt-get install -y linux-image-extra-$(uname -r)
&& apt-get install apt-transport-https ca-certificates
&& sudo apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
&& echo deb https://apt.dockerproject.org/repo ubuntu-trusty main >> /etc/apt/sources.list.d/docker.list
&& apt-get update && apt-get purge lxc-docker && apt-cache policy docker-engine && sudo apt-get install -y docker-engine
```
create a new subnet
```
docker network create --subnet=172.31.0.0/16 kenet
```
create image dataservice
```
docker build -f Dockerfile-dataservice -t ps/dataservice .
```
run a container
```
docker run --name dataservice --ip=172.31.0.10 --net-kenet -P --rm -it ps/dataservice
```

create image webservice
```
docker build -f Dockerfile-web -t ps/web .
```
create web container
```
docker run --name web --ip=172.31.0.11 --net=kenet -P --rm -it -- ps/web --dataservice=http://172.31.0.10:4000
```
