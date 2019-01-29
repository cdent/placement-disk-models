
This repo provides a few [gabbi](https://gabbi.readthedocs.io/)
files to demonstrate a variety of storage architectures that might
be made to work with nova compute. These are based on possible
scenarios when using vSphere clusters, where datastores can be
shared amongst clusters and there may be many of them.

These scenarios only demonstrate the placement side of the equation,
where the necessary functionality is already available. Work is
still needed on the nova side for some of this stuff to work but
a lot of the framework is in place.

You'll need to install gabbi (perhaps in a `virtualenv`):

```
pip install gabbi
```

To experiment with this you can use Docker, a sqlite database, and
[placedock](https://hub.docker.com/r/cdent/placedock/) which
provides a [placement
service](https://docs.openstack.org/placement).

The `dockerenv` file in the repo will help set up the placement
container with a sqlite database _in the container_. This means that
when you kill the container, you kill the data. In this case that's
what we want.

```
docker run -it --env-file dockerenv -p 8082:80 cdent/placedock:latest 
```

When that's up you can check it is working with:

```
curl -H 'x-auth-token: admin' http://localhost:8082/
```

That will output some JSON indicating the current placement
microversion. It should be `1.30` or greater.

The gabbi files are in the `gabbits` directory and can be run as
follows:

```
gabbi-run http://localhost:8082/ -- gabbits/<some file or wildcard>
```

Each file in the `gabbits` directory is extensively documented.
