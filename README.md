Patent Citation Data Loader
===========================

This repository contains all data needed to produce a Docker image that
contains a graph that describes citations between patents. The original
data has been taken from

  [https://snap.stanford.edu/data/cit-Patents.html](https://snap.stanford.edu/data/cit-Patents.html)

and from

  [http://www.nber.org/patents/](http://www.nber.org/patents/)

The only change is that all vertices have been explicitly added that are
end points of edges.

Creation of Docker image
------------------------

Use the following commands:

    docker build -t neunhoef/patentgraph .
    docker push neunhoef/patentgraph

Loading of data with Docker image
---------------------------------

To retrieve the data anywhere (where Docker is installed), use this command:

    docker run -it -v `pwd`:/data neunhoef/patentgraph

This will create four files in the current directory:

    patents.csv             - Vertices: Information about patents
    citations.csv           - Edges: Citation information
    patents_arangodb.csv    - Vertices: prepared for ArangoDB import
    citations_arangodb.csv  - Edges: prepared for ArangoDB import

To import, simply create a vertex collection `patents` and an edge
collection `citations` and use

    docker run -it -v `pwd`:/data arangodb/arangodb arangoimp --endpoint tcp://<IP>:<PORT> --collection patents --file /data/patents_arangodb.csv --type csv
    docker run -it -v `pwd`:/data arangodb/arangodb arangoimp --endpoint tcp://<IP>:<PORT> --collection citations --file /data/citations_arangodb.csv --type csv

