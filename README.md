<p align="center">
  <img height="100" src="https://raw.githubusercontent.com/pelias/design/master/logo/pelias_github/Github_markdown_hero.png">
</p>
<h3 align="center">A modular, open-source search engine for our world.</h3>
<p align="center">Pelias is a geocoder powered completely by open data, available freely to everyone.</p>
<p align="center">
<a href="https://github.com/pelias/api/actions"><img src="https://github.com/pelias/api/workflows/Continuous%20Integration/badge.svg" /></a>
<a href="https://en.wikipedia.org/wiki/MIT_License"><img src="https://img.shields.io/github/license/pelias/api?style=flat&color=orange" /></a>
<a href="https://hub.docker.com/u/pelias"><img src="https://img.shields.io/docker/pulls/pelias/api?style=flat&color=informational" /></a>
<a href="https://gitter.im/pelias/pelias"><img src="https://img.shields.io/gitter/room/pelias/pelias?style=flat&color=yellow" /></a>
</p>
<p align="center">
	<a href="https://github.com/pelias/docker">Local Installation</a> ·
        <a href="https://geocode.earth">Cloud Webservice</a> ·
	<a href="https://github.com/pelias/documentation">Documentation</a> ·
	<a href="https://gitter.im/pelias/pelias">Community Chat</a>
</p>
<details open>
<summary>What is Pelias?</summary>
<br />
Pelias is a search engine for places worldwide, powered by open data. It turns addresses and place names into geographic coordinates, and turns geographic coordinates into places and addresses. With Pelias, you’re able to turn your users’ place searches into actionable geodata and transform your geodata into real places.
<br /><br />
We think open data, open source, and open strategy win over proprietary solutions at any part of the stack and we want to ensure the services we offer are in line with that vision. We believe that an open geocoder improves over the long-term only if the community can incorporate truly representative local knowledge.
</details>

# Pelias API Server

This is the API server for the Pelias project. It's the service that runs to process user HTTP requests and return results as GeoJSON by querying Elasticsearch and the other Pelias services. This API is modified with a modified version of the [Pelias Query](https://ivv-devops.ivv.gruppe/T7.Software/Radroutenplaner_NRW/_git/Pelias-Query) to allow more fuzzy searches for the Radroutenplaner NRW.


## Using with pelias-query and Docker

If you'd like to run this API together with the `pelias-query` source code in the repository root, follow these steps.

- Clone this repository and enter it:

```bash
git clone ssh://ivv-devops.ivv.gruppe:22/T7.Software/Radroutenplaner_NRW/_git/Pelias-API
cd pelias-api
```

- Add the `pelias-query` repository next to this repo. You can either clone it directly (recommended for local development) or add it as a submodule (if you want the parent repo to track it):

Clone directly:

```bash
git clone ssh://ivv-devops.ivv.gruppe:22/T7.Software/Radroutenplaner_NRW/_git/Pelias-Query
```


- Build the Docker image from this repository root (where the `Dockerfile` lives):

```bash
docker build -t pelias-api:local .
```

- Run the container exposing the API port (defaults to `3100`) or pack it as a .tar file to ship it to the server. Mount or provide a config file via `PELIAS_CONFIG` or environment variables as needed. Example running with a local `pelias.json` config mounted:

```bash
docker run -d --name pelias-api \
  -p 3100:3100 \
  -e PORT=3100 \
  -e PELIAS_CONFIG=/config/pelias.json \
  -v "$(pwd)/pelias.json:/config/pelias.json:ro" \
  pelias-api:local
```

```bash
docker image save pelias-api:local > pelias-api-local.tar
```

Notes:
- If you cloned `pelias-query` directly into the `pelias-api` tree it will be a nested Git repository (not a submodule). The parent repo will treat `pelias-query/` as an untracked folder unless you add it as a submodule.
- Ensure any service dependencies (Elasticsearch, placeholder, libpostal, etc.) are available to the running container. In many setups you'll want to use Docker Compose or an external set of containers for those services.

