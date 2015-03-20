# Consul + CoreOS - hassles

This is for running a [Consul](http://consul.io/) cluster on [CoreOS](http://coreos.com) using etcd to bootstrap it.

## Why?

While CoreOS already has etcd built in and it is very similar to Consul, it is missing some of the service discovery features offered by Consul. Namely the service catalog and service health checking. These things are nice to have.

This also allows you to run [Registrator](https://github.com/progrium/registrator) on CoreOS clusters and still get the Consul-backed extra features (such as health-check support).

## Usage

1. Clone this repo: `git clone https://github.com/cap10morgan/consul-coreos.git`
1. Submit and run the included service file on your CoreOS cluster:

```
fleetctl submit consul@.service
fleetctl start consul@1
fleetctl start consul@2
... as many more as you want
```

Fleet will run one Consul container per CoreOS node, so you probably want to start a consul@N up to the number of CoreOS nodes in your cluster. Although be aware that currently this runs all of them as Consul servers, which is not recommended past a ceratin cluster size (some of them should start being run as clients past a certain number of nodes). Pull requests welcome!

## Implementation

It uses etcd to coordinate the bootstrapping of a Consul cluster on CoreOS. It runs Consul inside Docker containers (as nature intended) using [progrium's excellent Consul Docker image](https://github.com/progrium/docker-consul).

## TODO

1. Switch to adding client nodes (as opposed to more server nodes) when the cluster is above a certain size. For now this has been tested on a 3-node cluster and should work on clusters close to that size.

## License

BSD
