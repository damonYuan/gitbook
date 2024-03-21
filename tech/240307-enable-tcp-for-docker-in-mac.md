Enable TCP for docker in Mac
====

[Remote API with Docker for Mac](https://forums.docker.com/t/remote-api-with-docker-for-mac-beta/15639/2)

`socat -d TCP-LISTEN:2376,range=127.0.0.1/32,reuseaddr,fork UNIX:/var/run/docker.sock`
`socat TCP-LISTEN:2375,reuseaddr,fork UNIX-CLIENT:/var/run/docker.sock`
