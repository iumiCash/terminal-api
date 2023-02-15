# Authorization


To make requests to REST API you need to provide request headers

### Headers

???+ info "Header parameters"

    `Authorization` *string* **required**
    :    Separate your Base64-encoded **email** and **password** by a colon (:).
    
         Format:  `Basic {Your Base64-encoded email:password}` without curly braces.
         
         Example: `Basic Zm9vQGJhci5iYXo6d2Vha19wYXNzd29yZA==`
