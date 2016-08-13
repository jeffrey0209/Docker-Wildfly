Build image :
cd to this Dockerfile
docker build [Image's name]:[TAG] [.]

Run image :
docker run --name="[container's name]" -d -p 5678:8080 -p 9990:9990 -e WILDFLY_PASS="[password]"

Good to Go!
