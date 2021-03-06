# spark

An opinionated [Spark](http://spark.apache.org) based container with few additional tools:

* Scala
* Sbt
* AWS-CLI

Based on [gettyimages/spark](https://hub.docker.com/r/gettyimages/spark/) 

Intended to be used together with the examples from:
https://github.com/dataminelab/LearningSpark

Can be used to run standalone Spark or together with `docker-compose.yml` can create a local cluster.

## docker example

To run `SparkPi`, run the image with Docker:

    docker run --rm -it -p 4040:4040 dataminelab/dev-spark bin/run-example SparkPi 10

Before starting the docker daemon:
```
# Note we assume that the LearningSpark is checked out on the same level as docker-spark
git clone https://github.com/dataminelab/LearningSpark
```

To start the docker daemon:

```
# replace $PWD/LearningSpark with the full path to your LearningSpark
docker run -d --rm --name dev-spark -p 4040:4040 -p 8080:8080 -p 18080:18080 -v $PWD/LearningSpark:/var/examples dataminelab/dev-spark
# to see logs
docker logs -f dev-spark
# to connect with bash to the running container
docker exec -it dev-spark /bin/bash
```

Note, it might be useful to map ivy files to avoid downloading packages each time docker container is restarted:
```
mkdir $PWD/.sbt
mkdir $PWD/.ivy2
docker run -d --rm --name dev-spark -p 4040:4040 -p 8080:8080 -p 18080:18080 -v $PWD/LearningSpark:/var/examples -v $PWD/.ivy2:/root/.ivy2 -v $PWD/.sbt/:/root/.sbt/ dataminelab/dev-spark
```

See dockerhub:
https://hub.docker.com/r/dataminelab/dev-spark/

To build locally:

```
docker build -t dataminelab/dev-spark .
```

## Docker compose

To run a local cluster

To create a simplistic standalone cluster with docker-compose:

```
docker-compose up -d
```

The SparkUI will be running at http://${YOUR_DOCKER_HOST}:8080 with one worker listed. To run pyspark, exec into a container:

```
docker exec -it dockerspark_master_1 /bin/bash
bin/pyspark
```

To run SparkPi on docker cluster, exec into a container:

```
docker exec -it dockerspark_master_1 /bin/bash
bin/run-example SparkPi 10
```

