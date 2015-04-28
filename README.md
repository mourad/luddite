# Luddite service framework package

Luddite is a [golang][golang] package that provides a micro-framework
for RESTful web services.  It is built around extensible, pluggable
middleware layers and includes a standard `Resource` interface that
makes it easy to implement services that comply with the
[Orion REST API Standards][apistds].

[golang]: http://golang.org/
[apistds]: https://github.com/SpirentOrion/orion-docs/blob/master/api/api-standards.md

To run the example service:

    $ make restore all
    $ cd example
    $ ./example -c config.yaml

## Resources

Two types of resources are provided:

* Singleton: Supports `GET` and `PUT`.
* Collection: Supports `GET`, `POST`, `PUT`, and `DELETE`.

Resources may also implement `POST` actions and be made read-only.
Since `luddite` is a micro-framework, implementations retain
substantial flexibility.

## Middleware

Currently, the framework registers several middleware handlers for
each service:

* Bottom: Combines tracing, logging and recovery actions. Tracing
  generates a unique request id and optionally records traces to a
  persistent backend.  Logging logs requests/responses in an
  Apache-style log format.  Recovery handles panics that occur in HTTP
  method handlers and optionally includes stack traces in 500
  responses.
* Negotiation: Performs JSON (default) and XML content negotiation
  based on HTTP requests' `Accept` headers.
* Context: Performs API version selection and enforces the service's
  min/max supported version constraints.  Makes the `Service` instance
  and selected API version available to resource handlers as part of
  their dispatch [context][context].

[context]: http://blog.golang.org/context

# TODO

* Additional middleware handlers
  * JWT decode and validation
  * Request metrics / statsd integration
