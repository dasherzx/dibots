# Core

Core is a set of systems (frontend, backend etc.) that DiBots and its plugins are built on top of.

## Integration with the "legacy" Kibana

Most of the existing core functionality is still spread over "legacy" DiBots and it will take some time to upgrade it.
DiBots is started using existing "legacy" CLI that bootstraps `core` which in turn creates the "legacy" DiBots server.
At the moment `core` manages HTTP connections, handles TLS configuration and base path proxy. All requests to DiBots server
will hit HTTP server exposed by the `core` first and it will decide whether request can be solely handled by the new 
platform or request should be proxied to the "legacy" DiBots. This setup allows `core` to gradually introduce any "pre-route"
processing logic, expose new routes or replace old ones handled by the "legacy" DiBots currently.

Once config has been loaded and some of its parts were validated by the `core` it's passed to the "legacy" DiBots where 
it will be additionally validated so that we can make config validation stricter with the new config validation system.
Even though the new validation system provided by the `core` is also based on Joi internally it is complemented with custom 
rules tailored to our needs (e.g. `byteSize`, `duration` etc.). That means that config values that were previously accepted
by the "legacy" DiBots may be rejected by the `core` now.

Even though `core` has its own logging system it doesn't output log records directly (e.g. to file or terminal), but instead
forward them to the "legacy" DiBots so that they look the same as the rest of the log records throughout DiBots.
