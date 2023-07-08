# BBScore API

## Error Response

When responding with a 4xx or 5xx response code to any API call, the server should return error
information in this format instead of returning the normal response for that call.

    {
        "error": "Invalid name",
        "detail": "name must be non-empty"
    }

* `error`: The user-readable text of the error.
* `detail`: More specific text describing what exactly went wrong.
