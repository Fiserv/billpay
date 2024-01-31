## Versioning and Backwards Compatibility

All APIs will work as released until a breaking change is required, at
which time a new version, or alternative resource, will be created. The
existing resource will continue to be maintained until such time as
Fiserv announces that the API is deprecated and subsequently retired
with sufficient notice to clients that they can move to the new version.

### Backwards Compatibility

In order to limit change to consumers of the API, Fiserv will make every
effort to ensure backwards compatibility when new features are added.
This will be achieved by returning new properties and optional
inputs/query parameters. Clients need to understand the following
guidelines:

-   If a property is a primitive type and Fiserv documentation provides
    no value restrictions, a client MUST NOT assume any restrictions in
    values.

-   If a property is not declared as required to be returned, a client
    MUST NOT depend on it being present.

-   New properties may be added, but these new properties will not
    change how existing properties are used.

-   If new properties are added, the client MUST assume they can be
    returned, though the client need not use them.

-   Clients MUST provide default “fallback” error handling for
    unexpected error codes in all cases. Fiserv may return new error
    codes to allow for unexpected error conditions.

### Versioning

The APIs will be versioned with semantic versioning (semver.org). Fiserv
will increment a version number under the following rules:

-   MAJOR version when incompatible API changes are made.

-   MINOR version when functionality is added in a backwards-compatible
    manner.

-   PATCH version when backwards-compatible bug fixes are made.

Only major version changes will eventually require depreciation of
previous versions.