# My Docker stack
Docker compose application with multiple files to bring up everything I need on one docker machine.
## Short intro & motivation
In 2024 I immersed in the study of Docker. As I studied it, Docker began to seem very convenient to me, and I started to use it for my needs. In a short time, I had several Docker Compose apps served by one nginx front-end, so I decided that it will be a good idea to manage them together.

I also decided to make this repository public in case someone wants to use some features that I implemented in this stack. 
## Bring the stack up
Firstly, clone the repo and rename `.env.sample` file:
```bash
git clone --recurse-submodules https://github.com/sanchpet/my_docker_stack.git
cd my_docker_stack
mv .env.sample .env
```
Then you have to edit `.env` file to set up the parameters.

You may notice that every app uses same external network. I made it external because it is simple to manage. Shared network is essential for reverse proxy to work. 

Initialize network:
```bash
docker network create nginx-proxy-net
```
And set the value of the `PROXY_NET` variable in `.env` file same as network name.

At last, start bring the stack up!
```bash
docker compose -f docker-compose.yml -f vaultwarden/docker-compose.yaml -f links-website/docker-compose.yml up -d
```

## Base application (nginx-proxy + acme-companion)
This is the framework of the whole stack. You can see a detailed explanation how it works on the [project repo](https://github.com/nginx-proxy/acme-companion). 

The key feature is that I can define `VIRTUAL_HOST` and `LETSENCRYPT_HOST` environment variable for other Docker service that exposes a port, and it will be automatically served with nginx front-end and LE SSL certificate using `docker-gen`. 

I simply put 2 variables in an environment and get nginx configured with https, it's pure magic!

## Vaultwarden
Self-hosted password manager. [Project page](https://github.com/dani-garcia/vaultwarden)

If you mount an empty directory (in my case it is `/opt/vaultwarden/`), a new vaultwarden database will be initialized. As it is sensitive data, I decided to keep it out of the repo. 

In the near future, I plan to add to this app a service to back up the vaultwarden database to a desired location.

## Links-website
My website with personal links made with [Hugo](https://github.com/gohugoio/hugo) and [Lynx](https://github.com/jpanther/lynx) template.

I found out about Hugo and fell in love with it, so I recommend you to check out it too. Simplest way to run pretty website in Docker.