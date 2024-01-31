## Requirements and Limitations

-   The minimum TLS version is 1.2.

-   All API requests must be made over HTTPS. Calls made over plain HTTP
    will fail. API requests without authentication will also fail.

For more information, see *CheckFree Next Design Requirements, Best
Practices, and Performance Considerations*.

### Caching

In order to deliver an efficient implementation, clients must implement
caching. Data returned from GET operations must be cached within the
client’s implementation.

When data is changed, clients should invalidate all appropriate
resources that have been cached. As an example, after a POST
transaction, Payees, Transactions, Bank Accounts, Bills, and Todos may
all be invalidated.

### FraudNet Data Requirements

Clients using FraudNet must provide the following for each session:
user's IP address, user-agent, and channel.

#### IP Address and User-Agent

If calls are originating directly from an end-user device (via native
app or JavaScript calls), the IP address and user-agent data will be
populated automatically.

For calls originating from a client server, the following HTTP headers
should be set:

-   Source-Ip-Address: This should be the IP address of the device that
    the user was originating bill pay transactions from.

-   User-Agent: This is the standard user-agent field and should be
    overridden with the user agent of the device/client the user was
    originating bill pay transactions from.

#### Channel

In the JWT used to obtain an access token, the client must include
fiserv.identity.billpay.channel in the payload. The valid values are
Desktop and Mobile.
