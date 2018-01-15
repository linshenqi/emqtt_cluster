# emqtt_cluster
Emqtt cluster with haproxy as well as confd and etcd

## Summary
* Build the emqtt docker according to [emq-docker](https://github.com/emqtt/emq-docker)
and you can get my prebuilt image by the following command:
```
docker pull linshenqi/emqtt:2.3.3
```

* As we use confd for rendering config file of haproxy, I make a new haproxy docker image with confd. And you can get it by the following command:
```
docker pull linshenqi/haproxy_confd
```
Also the Dockerfile can be found [HERE](https://github.com/linshenqi/haproxy_confd)

## Usage
#### *Docker*
We provider a docker-compose.yml file to interact with the cluster.
Let's take a look at the docker-compose.yml. As you can see it contains 3 services which are haproxy, emqttd and etcd. You should pay some attention to the exposed ports in haproxy and etcd to avoid port confliction on your host. The emqttd's ports will be exposed randomly as it can be scaled as you like.
Beside we persist some volumes in the service haproxy. Before you run up the cluster you should create some dirs for the config files according to the compose file on your host. We provider 3 basic config files under the 'configs' directory.

* **start the cluster with one emqttd node**
```
docker-compose up -d
```
If everything is OK, you will find the haproxy.cfg in your local path '/data/emqtt/haproxy/conf'(this path may be different according to the docker-compose.yml). This cfg file is rendered by the confd automatically.
Now you can open the url 'http://127.0.0.1:1080/'(the port of the url may be different according to the docker-compose.yml) in your browser and you may find one emqttd node on the dashboard.
The default user:pwd of the dashboard is admin:admin as it can be found in the haproxy.cfg.toml.

* **or start the cluster with two emqttd node**
```
docker-compose up -d --scale emqttd=2
```

* **scale up the emqttd service with four nodes**
```
docker-compose scale emqttd=4
```
Now you can refresh the browser and find the 4 emqttd nodes on the dashboard

* **shut down everything**
```
docker-compose down
```
