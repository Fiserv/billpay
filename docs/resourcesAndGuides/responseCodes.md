## HTTP Response Codes

CheckFree Next APIs use standard HTTP response codes to indicate the
success or failure of an API request.

| HTTP Status Code            | Description                                                                                                                                                                                |
|------------|------------------------------------------------------------|
| 200 - OK                    | Standard response for successful HTTP requests.                                                                                                                                            |
| 201 - Created               | The request has been fulfilled, resulting in the creation of a new resource.                                                                                                               |
| 204 - No content            | The server successfully processed the request, but is not returning any content.                                                                                                           |
| 307 - Redirect              | The requested resource resides temporarily under a different URI.                                                                                                                          |
| 400 - Bad Request           | The server cannot process the request due to an apparent client error. We will also return the specific reason why you received this error based on the action you were trying to perform. |
| 401 - Unauthorized          | Authentication is required and has failed or has not yet been provided (for example, an invalid key).                                                                                      |
| 403 - Forbidden             | The request was valid, but the server is refusing action.                                                                                                                                  |
| 404 - Not found             | The server could not find what was requested.                                                                                                                                              |
| 500 - Internal server error | Internal server error      