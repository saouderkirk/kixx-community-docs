# Request Handlers

Request handlers are the core building blocks for processing HTTP requests in Kixx.

## Handler Signature

All handlers follow the same function signature:

```javascript
async function handler(context, request, response, skip) {
    // Process request
    // Return response (modified or unchanged)
}
```

**Parameters**:
- `context` - Application context with services, config, logger, paths
- `request` - HTTP request object with URL, headers, method, body
- `response` - HTTP response object to modify and return
- `skip` - Function to call when this handler has served the request (skips remaining handlers)

**Return value**: Always return the `response` object (modified or unchanged).

## Built-in Handlers

### StaticFileServer

Serves files from the `/public` directory.

```javascript
import StaticFileServer from 'kixx/lib/request-handlers/handlers/static-file-server.js';

// Default usage (uses /public directory)
StaticFileServer()

// Custom options
StaticFileServer({
    publicDirectory: '/custom/path',
    cacheControl: 'public, max-age=3600'
})
```

**Behavior**:
1. Validates request path (security checks)
2. Looks for file in public directory
3. If file exists: serves it with proper Content-Type, calls `skip()`
4. If file doesn't exist: returns response unchanged (next handler runs)

### PageHandler

Renders pages from the `/pages` directory using templates.

```javascript
import PageHandler from 'kixx/lib/request-handlers/handlers/page-handler.js';

// Default usage
PageHandler()

// Custom options
PageHandler({
    pathname: '/custom-page',      // Override URL pathname
    contentType: 'text/html',      // Override content type
    baseTemplateId: 'custom.html'  // Override template
})
```

**Behavior**:
1. Loads page metadata from `pages/{path}/page.jsonc`
2. Loads page content from `pages/{path}/page.md` or `.html`
3. Processes metadata templates (title, description)
4. Renders content through base template
5. Returns HTML response

### PageErrorHandler

Handles errors and renders error pages.

```javascript
import PageErrorHandler from 'kixx/lib/request-handlers/error-handlers/page-error-handler.js';

PageErrorHandler()
```

**Behavior**:
- Catches `NotFoundError` → renders 404 page
- Catches other errors → renders 500 page
- Uses error templates from `/pages` if available

## Creating Custom Handlers

### Basic Handler Factory

```javascript
// handlers/my-handler.js
export default function MyHandler(options = {}) {
    return async function myHandler(context, request, response, skip) {
        const logger = context.logger.createChild('MyHandler');

        // Access config
        const config = context.config.getNamespace('MyApp.MyHandler');

        // Check request
        if (request.method !== 'POST') {
            return response; // Let next handler try
        }

        // Do something
        logger.info('Processing request', { url: request.url.pathname });

        // Respond
        skip(); // Signal we're handling this request
        return response.respondWithJSON(200, { success: true });
    };
}
```

### Handler with Services

```javascript
export default function DataHandler() {
    return async function dataHandler(context, request, response, skip) {
        // Get a registered service
        const viewService = context.getService('kixx.AppViewService');

        // Use the service
        const data = await viewService.getPageData(request.url.pathname);

        skip();
        return response.respondWithJSON(200, data);
    };
}
```

## Request Object

```javascript
request.method           // 'GET', 'POST', etc.
request.url.pathname     // '/about'
request.url.hostname     // 'example.com'
request.url.searchParams // URLSearchParams object
request.headers.get('content-type')
request.hostnameParams   // From virtual host pattern matching
request.pathnameParams   // From route pattern matching (e.g., { slug: 'my-post' })
request.isHeadRequest()  // true if HEAD request
request.isJSONRequest()  // true if Accept: application/json
```

## Response Object

```javascript
// Set headers
response.headers.set('X-Custom', 'value')

// JSON response
response.respondWithJSON(200, { data: 'value' }, { whiteSpace: 2 })

// HTML response
response.respondWithHTML(200, '<html>...</html>')

// Stream response (for files)
response.respondWithStream(200, contentLength, readStream)

// Not modified (304)
response.respondNotModified()

// Access response properties
response.props  // Shared props object (passed between handlers)
```

## Context Object

```javascript
context.config.getNamespace('Kixx.StaticFileServer')  // Get config section
context.logger.createChild('MyHandler')               // Create child logger
context.paths.public_directory                        // Path to /public
context.paths.root_directory                          // Project root
context.getService('kixx.AppViewService')             // Get registered service
```

## Handler Chain Execution

Handlers execute in array order:

```jsonc
{
    "handlers": [
        "StaticFileServer()",  // 1. Tries to serve static file
        "PageHandler()"        // 2. Runs if StaticFileServer didn't serve
    ]
}
```

**Flow**:
1. Handler 1 runs
2. If Handler 1 calls `skip()`, remaining handlers don't run
3. If Handler 1 returns response unchanged, Handler 2 runs
4. Process continues until a handler serves or all handlers complete
5. If no handler serves, error handlers run (404)

## TODO

- Document middleware (inbound/outbound phases)
- Document error handler chain
- Document custom service registration
- Add examples for form handling, API endpoints
