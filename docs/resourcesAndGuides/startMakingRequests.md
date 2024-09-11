## Start Making Requests

After authentication you will be given an access token and a refresh
token. The access token should be used in the authorization header:
"Authorization: Bearer &lt;access\_token&gt;".

The refresh token should be used within the configurable refresh
interval (set during client implementation, default 10 minutes).