
# Socket.IO Chat with nginx & redis adapter

A simple chat demo for socket.io

## How to use

```
$ docker-compose up -d
```

And then point your browser to `http://localhost:8000`.

This will start four Socket.IO nodes, behind a nginx proxy which will loadbalance the requests (using the IP of the client, see [ip_hash](http://nginx.org/en/docs/http/ngx_http_upstream_module.html#ip_hash)).

Each node connects to the redis backend, which will enable to broadcast to every client, no matter which node it is currently connected to.

```
# you can kill a given node, the client should reconnect to another node
$ docker-compose stop s1
```

A `client` container is included in the `docker-compose.yml` file, in order to test the routing.

You can create additional `client` containers with:

```
$ docker-compose scale client=10
$ docker-compose logs client
```

## Features

- Multiple users can join a chat room by each entering a unique username
on website load.
- Users can type chat messages to the chat room.
- A notification is sent to all users when a user joins or leaves
the chatroom.

## inspect redis datas

```
docker exec -ti redis bash
redis-cli
127.0.0.1:6379> PSUBSCRIBE *
2) "*"
3) (integer) 1


1) "pmessage"
2) "*"
3) "socket.io#/#"
4) "\x93\xa6aD4JI5\x83\xa4type\x02\xa4data\x92\xabuser joined\x82\xa8username\xa4dezd\xa8numUsers\x01\xa3nsp\xa1/\x83\xa5rooms\x90\xa6except\x91\xb4aS0NlnDYzoOX-YClAAAF\xa5flags\x80"
```
