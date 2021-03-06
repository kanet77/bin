# HyperREST bin

A simple HTTP service with controlled behaviour, inspired by [httpbin](https://github.com/kennethreitz/httpbin).

The service makes use of HTTP headers, and predominantly [X-Prefer](http://tools.ietf.org/html/draft-snell-http-prefer-18), in order to allow the client to control server behaviour.

Additionaly it also makes use of [know-your-http-well](https://github.com/andreineculau/know-your-http-well) and acts as a bookmarking tool for specification of HTTP status codes, headers, methods and relations.

This project is live at [bin.hyperrest.com](http://bin.hyperrest.com) and has a [search engine](http://mycroftproject.com/search-engines.html?name=bin.hyperrest.com) for convenience.


## Install & run

```bash
npm install hyperrest-bin
hyperrest-bin                                                                         # or PORT=1337 hyperrest-bin
```


## Usage

```bash
curl                                                          http://127.0.0.1:1337   # README.md
curl                                                          http://127.0.0.1:1337/* # README.md on any path
curl -XPOST                                                   http://127.0.0.1:1337   # README.md
curl -XPOST -H"Accept: application/json"                      http://127.0.0.1:1337   # JSON TRACE
curl -XPOST -H"Accept: application/xml"                       http://127.0.0.1:1337   # XML TRACE

                                                                                      # TRACE, METHOD OVERRIDE
curl -XTRACE                                                  http://127.0.0.1:1337   # message/http TRACE
curl -XPOST  -H"X-HTTP-Method-Override: TRACE"                http://127.0.0.1:1337   # message/http TRACE still
curl -XTRACE -H"Accept: application/json"                     http://127.0.0.1:1337   # JSON TRACE
curl -XTRACE -H"Accept: application/xml"                      http://127.0.0.1:1337   # XML TRACE

                                                                                      # ORIGINATING IP
                                                                                      # see response

                                                                                      # GZIP/DEFLATE
curl -H"Accept-Encoding: gzip,deflate"                        http://127.0.0.1:1337   # GZIP README.md

                                                                                      # PREFER (as per registered preferences)
curl -H"X-Prefer: status=404"                                 http://127.0.0.1:1337   # 404 Not Found
curl -H"X-Prefer: wait=10"                                    http://127.0.0.1:1337   # wait 10 seconds, then README.md
curl -H"X-Prefer: return-minimal"                             http://127.0.0.1:1337   # 200 OK, but no README.md
curl -H"X-Prefer: return-representation" -H"Accept:text/html" http://127.0.0.1:1337   # return README.md by force

                                                                                      # PREFER (as per extensions of preferences)
curl -H"X-Prefer: cookie=name1|v, cookie=name2|v"             http://127.0.0.1:1337   # set cookies "name1" and "name2"
curl -H"X-Prefer: cookie=name1"                               http://127.0.0.1:1337   # delete cookie "name1"

                                                                                      # PREFER response as defined in the request body
curl -XPOST \
     -H"Content-Type: application/json" \
     -H"X-Prefer: return-request" \
     -d'{"status":"200", \
         "headers":{"Content-Type":"text/plain"}, \
         "body":"TEXT"
        }'                                                    http://127.0.0.1:1337   # return 200, etc.


                                              # https://github.com/andreineculau/know-your-http-well
curl http://127.0.0.1:1337/method/{value}     # Specification for HTTP Method
curl http://127.0.0.1:1337/header/{value}     # Specification for HTTP Header
curl http://127.0.0.1:1337/statusCode/{value} # Specification for HTTP Status Code
curl http://127.0.0.1:1337/rel/{value}        # Specification for HTTP Relation
curl http://127.0.0.1:1337/spec/{value}       # Specification for HTTP AnyOfTheAbove (I feel lucky mode)
```


## License

[Apache 2.0](LICENSE).
