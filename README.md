docker-drupal-nginx
===================

This repo contains a recipe for making a [Docker](http://docker.io) container for Drupal, using Linux, Nginx, php-apc, php-fpm and MySQL. 
To build, make sure you have Docker [installed](http://www.docker.io/gettingstarted/), 

## Install docker:
```
sudo apt-get -y install lxc-docker
curl get.docker.io | sudo sh -x
```

## Installation

```
$ git clone https://github.com/klokie/docker-drupal-nginx.git
$ cd docker-drupal-nginx
$ sudo docker build -t="docker-drupal-nginx" .
```

## And run the container, connecting port 80:
```
sudo docker run -d -t -p 80:80 docker-drupal-nginx
```
That's it!
Visit http://localhost/ in your webrowser. 

Note: you cannot have port 80 already used or the container will not start.
In that case you can start by setting: `-p 8080:80`


### Credentials

* ROOT   MYSQL_PASSWORD will be on /mysql-root-pw.txt
* DRUPAL MYSQL PASSWORD will be on /drupal-db-pw.txt
* Drupal account-name=admin & account-pass=admin


## More docker awesomeness

This will create an ID that you can start/stop/commit changes:
```
# sudo docker ps
ID                  IMAGE                   COMMAND               CREATED             STATUS              PORTS
538114c20d36        docker-drupal-nginx   /bin/bash /start.sh   3 minutes ago       Up 6 seconds        80->80  
```

Start/Stop
```
sudo docker stop 538114c20d36
sudo docker start 538114c20d36
```

Commit the actual state to the image
```
sudo docker commit 538114c20d36 docker-drupal-nginx
```

Starting again with the commited changes
```
sudo docker run -d -t -p 80:80 docker-drupal-nginx /start.sh
```

Shipping the container image elsewhere 
```
sudo docker push  docker-drupal-nginx
```

You can find more images using the [Docker Index][docker_index].


### Clean up
While i am developing i use this to rm all old instances
```
sudo docker ps -a | awk '{print $1}' | xargs -n1 -I {} docker rm {}
``` 

### Known Issues
* Upstart on Docker is broken due to [this issue][docker_upstart_issue], and that's one of the reasons the image is puppetized using vagrant.
* Warning: This is still in development and ports shouldn't be open to the outside world.


## Contributing
Feel free to fork and contribute to this code. :)

1. Fork the repo
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Added some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request

## Authors

Forked from/based on the [docker-drupal-nginx](https://github.com/ricardoamaro/docker-drupal-nginx) project by [Ricardo Amaro](https://github.com/ricardoamaro) (<mail@ricardoamaro.com>)

## License
GPL v3

[author]:                 https://github.com/ricardoamaro
[docker_upstart_issue]:   https://github.com/dotcloud/docker/issues/223
[docker_index]:           https://index.docker.io/
