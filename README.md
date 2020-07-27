# poc-stendhal-elk

These are the steps to run the elastic stack:

## Clone
First of all clone the repository:

```bash
git clone https://musashi.lkmx.io/stendhal-marketing-platform/poc-stendhal-elk
```
Change the name of the **.env.default** file in the root directory to **.env**
You can specify your credentials in:

```bash
ELASTIC_USERNAME=elastic
ELASTIC_PASSWORD=password
```

## Set up
Create the **keystore** and **certs** (Run only once) 
You need to create these  files in order to add security to you ELK stack. The first command creates a Keystore it then adds `$ELASTIC_PASSWORD` as the default superuser `elastic` and the second one creates a self-signed single `PKCS#12 keystore` that includes the node certificate, node key, and CA certificate.

```bash
docker-compose -f docker-compose.setup.yml run --rm keystore
docker-compose -f docker-compose.setup.yml run --rm certs
```
This will create the container and removes them as they finish to save space. Saving its output to `/secrets` directory.


## Deploy
And then start elastic stack
```bash
docker-compose up -d
```
To test it, go to [Kibana](localhost:5601) and enter your {user} and {password}

## Troubleshooting


* If your [Kibana](localhost:5601) page doesn't start, you can see the logs using:
```bash
docker-compose logs -f
```

* In Ubuntu: If the logs in elastic node says: *vm.max_map_count [value] is too low* try to increase your virtual memory:
```bash
sysctl -w vm.max_map_count=262144
```

* If the logs in kibana node says: *Another Kibana instance appears to be migrating the index* try to delete all the kibana indexes and deploy again.
```bash
curl -XDELETE http://localhost:9200/* -u user:password
```
