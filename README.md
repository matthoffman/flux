# flux

A Clojure based Solr client. Current version support is `4.6.0`.

## Usage

###Http

```clojure
(require '[flux.http :as http])

(def conn (http/create "http://localhost:8983/solr" :collection1))
```

###Embedded

```clojure
(require '[flux.embedded :as embedded])

(def cc (embedded/create-core-container "path/to/solr-home" "path/to/solr.xml"))
```

####Core auto-discovery
Flux also supports `core.properties`. Just give `create-core` the solr-home path as the only argument and Flux will use the `org.apache.solr.core.CoresLocator` object for discovering cores.
Note: It's important to call the `load` method on the resulting `CoreContainer` instance:

```clojure
(def cc (doto (embedded/create-core-container "path/to/solr-home")
              (.load))
```

Now create the embedded server instance:

```clojure
(def conn (embedded/create cc :collection1))
```

###Client
Once a connection as been created, use the `with-connection` macro to wrap client calls:

```clojure
(require '[flux.core :as flux])

(flux/with-connection conn
    (flux/add [{:id 1} {:id 2}])
    (flux/commit)
	(flux/query "*:*"))
```

###javax.servlet/servlet-api and EmbeddedSolrServer

Unfortunately, EmbeddedSolrServer requires javax.servlet/servlet-api as an implicit dependency. Because of this, Flux adds this lib as a depedency.

  * http://wiki.apache.org/solr/Solrj#EmbeddedSolrServer
  * http://lucene.472066.n3.nabble.com/EmbeddedSolrServer-java-lang-NoClassDefFoundError-javax-servlet-ServletRequest-td483937.html

###Test
```shell
lein midje
```

## License

Copyright © 2013 Matt Mitchell

Distributed under the Eclipse Public License, the same as Clojure.
