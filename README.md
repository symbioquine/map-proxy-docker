# map-proxy-docker

Run [MapProxy](https://mapproxy.org/) as a docker container.

## Getting Started

Create your configs according to the [MapProxy documentation](https://mapproxy.org/docs/1.12.0/).

For this example we'll use the simple configuration from the documentation;

```sh
mkdir map-proxy-config/ && curl https://mapproxy.org/docs/1.12.0/_downloads/simple_conf.yaml > map-proxy-config/mapproxy.yaml
```

Start the proxy;

```sh
docker run --name=map-proxy-server --rm -v $(pwd)/map-proxy-config:/mnt/config -p 5807:5807 -it symbioquine/map-proxy:1.12.0 serve-develop /mnt/config/mapproxy.yaml --bind 0.0.0.0:5807
```

MapProxy should now be running at http://localhost:5807/demo/

### In docker-compose.yml

Add the `map-proxy` service to your docker-compose.yml file;

```yaml
  map-proxy:
    image: symbioquine/map-proxy:1.12.0
    command: serve-develop /mnt/config/mapproxy.yaml --bind 0.0.0.0:5807
    volumes:
      - './map-proxy-config:/mnt/config'
    ports:
      - '5807:5807'
```

### Seeding

Assuming your `seed.yaml` file has been created based on [the seeding documentation](https://mapproxy.org/docs/1.12.0/seed.html) and is located at `map-proxy-config/seed.yaml`.

```bash
docker run --name=map-proxy-seeder --rm -v $(pwd)/map-proxy-config:/mnt/config --entrypoint mapproxy-seed -it symbioquine/map-proxy:1.12.0 -f /mnt/config/mapproxy.yaml -c 16 /mnt/config/seed.yaml
```

## Building

Build the image from the git repository locally;

```bash
docker build -t map-proxy:dev src/
```

Then replace `symbioquine/map-proxy:1.12.0` with `map-proxy:dev` in the above instructions.
