# War Eagle Postman Collections

Centralized Postman collections and environments for all War Eagle Azure Function applications.

## Overview

This repository contains:
- **Collections**: One Postman collection per Azure Function app
- **Environments**: Shared environments (Dev, UAT, Prd, Global) used across all collections

## Structure

```
mdsi-war-eagle-postman/
├── collections/
│   ├── BoJackson.postman_collection.json
│   ├── CamNewton.postman_collection.json
│   └── [other function app collections...]
├── environments/
│   ├── War-Eagle-Dev.postman_environment.json
│   ├── War-Eagle-UAT.postman_environment.json
│   ├── War-Eagle-Prd.postman_environment.json
│   ├── War-Eagle-Local.postman_environment.json
│   └── War-Eagle-Global.postman_environment.json
└── README.md
```

## Getting Started

### 1. Import Collections and Environments

1. Open Postman
2. Click **Import** in the top left
3. Import all collections from the `collections/` folder
4. Import all environments from the `environments/` folder

### 2. Select an Environment

In Postman's top-right corner:
1. Select the appropriate environment (Dev, UAT, or Prd)
2. Optionally enable the Global environment for shared variables

### 3. Configure Environment Variables

Each environment requires configuration:

#### Dev/UAT/Prd/Local Environments

**Bo Jackson variables:**
- `baseUrl` - Azure Function App URL (e.g., `https://func-bojackson-dev.azurewebsites.net`)
- `functionKey` - Function authorization key (obtain from Azure Portal)

**Cam Newton variables:**
- `camNewtonBaseUrl` - Azure Function App URL (e.g., `https://func-camnewton-dev.azurewebsites.net`)
- `camNewtonFunctionKey` - Function authorization key (obtain from Azure Portal)

#### Global Environment
- `accessToken` - Shared access tokens (D365, etc.)
- Other shared variables used across multiple function apps

**Note**: Fill in the `functionKey` values manually after importing. These are NOT checked into source control.

## Collections

### Bo Jackson (`BoJackson.postman_collection.json`)

Purchase Order Receipt integration service - polls D365 for new purchase order receipts and processes them.

**Endpoints:**
- **Purchase Order Receipt:**
  - `POST /api/purchaseOrderReceipt/poll` - Manually trigger the PO receipt poller
- **Diagnostics:**
  - `GET /api/diagnostics/heartbeat` - Health check
  - `GET /api/diagnostics/version` - Get API version
  - `GET /api/diagnostics/apiName` - Get API name
  - `GET /api/diagnostics/exceptionTest` - Test exception handling
  - `GET /api/diagnostics/customHttpStatus/{statusCode}/{message}` - Test custom HTTP status
  - `POST /api/diagnostics/logTest` - Test logging functionality

### Cam Newton (`CamNewton.postman_collection.json`)

Purchase Order Receipt notification queue processor - processes PO receipt notifications from Service Bus.

**Endpoints:**
- **Diagnostics:**
  - `GET /api/diagnostics/heartbeat` - Health check
  - `GET /api/diagnostics/version` - Get API version
  - `GET /api/diagnostics/apiName` - Get API name
  - `GET /api/diagnostics/exceptionTest` - Test exception handling
  - `GET /api/diagnostics/customHttpStatus/{statusCode}/{message}` - Test custom HTTP status
  - `POST /api/diagnostics/logTest` - Test logging functionality

**Note:** Cam Newton's primary function is a Service Bus queue processor (not HTTP triggered), so it only exposes diagnostic endpoints.

## Environment Variables Reference

### Local Environment
- `baseUrl`: `http://localhost:7071`
- `functionKey`: `na` (not required for local)
- `camNewtonBaseUrl`: `http://localhost:7071`
- `camNewtonFunctionKey`: `na` (not required for local)

### Dev Environment
- `baseUrl`: `https://func-bojackson-dev.azurewebsites.net`
- `functionKey`: `[YOUR_DEV_BOJACKSON_FUNCTION_KEY]`
- `camNewtonBaseUrl`: `https://func-camnewton-dev.azurewebsites.net`
- `camNewtonFunctionKey`: `[YOUR_DEV_CAMNEWTON_FUNCTION_KEY]`

### UAT Environment
- `baseUrl`: `https://func-bojackson-uat.azurewebsites.net`
- `functionKey`: `[YOUR_UAT_BOJACKSON_FUNCTION_KEY]`
- `camNewtonBaseUrl`: `https://func-camnewton-uat.azurewebsites.net`
- `camNewtonFunctionKey`: `[YOUR_UAT_CAMNEWTON_FUNCTION_KEY]`

### Prd Environment
- `baseUrl`: `https://func-bojackson-prd.azurewebsites.net`
- `functionKey`: `[YOUR_PRD_BOJACKSON_FUNCTION_KEY]`
- `camNewtonBaseUrl`: `https://func-camnewton-prd.azurewebsites.net`
- `camNewtonFunctionKey`: `[YOUR_PRD_CAMNEWTON_FUNCTION_KEY]`

### Global Environment
- `accessToken`: Shared access tokens set dynamically
- Common configuration values that span all environments

## Adding New Collections

When creating a new Azure Function app:

1. Create a new collection file: `collections/[FunctionAppName].postman_collection.json`
2. Use environment variables for all environment-specific values:
   - `{{baseUrl}}` for the base URL
   - `{{functionKey}}` for authorization
   - `{{accessToken}}` or other Global variables for shared tokens
3. Document the new collection in this README

## Security Notes

- **Never commit function keys or access tokens** to this repository
- Function keys should be stored in environment variables with empty default values
- Access tokens should be set in the Global environment at runtime
- Use environment variables for all sensitive data

## Contributing

1. Keep collection organization consistent
2. Use descriptive request names
3. Add tests to validate responses
4. Document new collections in this README
5. Ensure all sensitive values use environment variables
