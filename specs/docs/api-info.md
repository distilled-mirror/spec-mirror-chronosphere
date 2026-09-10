> ## Documentation Index
> Fetch the complete documentation index at: https://docs.chronosphere.io/llms.txt
> Use this file to discover all available pages before exploring further.

# Get started with the Chronosphere API

> How to access and use the Chronosphere API for programmatic access to your observability data.

The Chronosphere API is an HTTP/JSON REST API for Chronosphere Observability
Platform, where the HTTP method and the URL path define endpoints. To access the
Chronosphere API, you need an *API token*.

For information about using the Prometheus API, see
[Prometheus API overview](/tooling/prometheus-api#prometheus-http-api).

For API endpoint definitions, see the [API reference documentation](/tooling/api-info/definition).
For the structure of dashboard configurations, see the
[dashboard configuration schema](/tooling/api-info/dashboard_schema).

<Note>
  Chronosphere supports only publicly documented API endpoints. Undocumented, private,
  or experimental API endpoints might change without warning.
</Note>

## Create an API token

A service can access the Chronosphere API by authenticating with its API token.
Many Chronosphere API requests require an unrestricted service account. For details,
see [Service accounts](/administer/accounts-teams/service-accounts).

A user with sufficient permissions can access the Chronosphere API by creating a
temporary personal access token. For details, see
[Personal access tokens](/administer/accounts-teams/personal-access-tokens).

## Send requests

Because the Chronosphere API requires authentication, include an API token with your
`curl` request, as shown in the following example. For more details, see
[Create an API token](/tooling/api-info#create-an-api-token).

```shell /"TOKEN"/ /INSTANCE/ /METHOD/ /ENDPOINT_PATH/ theme={null}
export CHRONOSPHERE_API_TOKEN="TOKEN"
export CHRONOSPHERE_DOMAIN="INSTANCE.chronosphere.io"

curl -H "API-Token: ${CHRONOSPHERE_API_TOKEN}" \
     -X METHOD "https://${CHRONOSPHERE_DOMAIN}/ENDPOINT_PATH"
```

Replace the following:

* *`TOKEN`*: Your API token.
* *`INSTANCE`*: The subdomain name for your organization's Observability Platform instance.
* *`METHOD`*: The HTTP method to use with the request, such as `GET` or `POST`.
* *`ENDPOINT_PATH`*: The specific endpoint you want to access.

<Note>
  [Chronoctl](/tooling/chronoctl) also uses the environment variable `CHRONOSPHERE_API_TOKEN`.
</Note>

For example, to list all collections:

```shell theme={null}
curl -H "API-Token: ${CHRONOSPHERE_API_TOKEN}" \
     -X GET \
     "https://${CHRONOSPHERE_DOMAIN}/api/v1/config/collections"
```

To create a collection, send a POST request with JSON body:

```shell theme={null}
curl -H "API-Token: ${CHRONOSPHERE_API_TOKEN}" \
     -X POST \
     --data '{"collection": {"name": "My Test Collection"}}' \
     "https://${CHRONOSPHERE_DOMAIN}/api/v1/config/collections"
```

## Use an API token

You can use an API token to make HTTP API calls.

For HTTP calls, use the `Authorization` header with the `Bearer` token authentication
scheme.

For example, to use the API token to call the Query API with `curl`, run:

```shell wrap theme={null}
curl -H "Authorization: Bearer ${CHRONOSPHERE_API_TOKEN}" "https://${CHRONOSPHERE_DOMAIN}.chronosphere.io/data/metrics/api/v1/query?query=up"
```

### Change Actors

APIs that modify configuration (for example: monitors, dashboards) accept a header
`Chronosphere-Actor`. If set, its value will be displayed in version history alongside the
service account the API token belongs to. This lets you propagate who triggered a change
in addition to the service account that made it, supporting GitOps and agentic workflows
where a service account is acting on behalf of another user.

## Pagination

All endpoints that list objects are *paginated*, which means the server can
potentially return only a subset, or page, of the requested objects, along with an
opaque page token the client can use to fetch the next page of objects. To request
the first page, provide an empty page token. If there are no more pages to request,
the server returns an empty page token.

If you use filter arguments, the client must pass the same filters on each page request
in a single iteration. If you use new filter arguments, restart pagination by requesting
the first page with the new filters.

This example demonstrates the process to iterate over every monitor:

1. Request the first page by sending a plain request with no page token:

   ```shell wrap theme={null}
   curl -H "API-Token: ${CHRONOSPHERE_API_TOKEN}" "https://${CHRONOSPHERE_DOMAIN}/api/v1/config/monitors"
   ```

   The server responds with:

   ```json theme={null}
   {
     "page": {
       "next_token": "abc123"
     },
     "monitors": [
       {
         "slug": "my-monitor",
         "name": "My Monitor",
         // ...
       },
       // ...
     ]
   }
   ```

2. The response has a non-empty `page.next_token` field, which means you must send
   another request with this page token to fetch the next page of monitors:

   ```shell wrap theme={null}
   curl -H "API-Token: ${CHRONOSPHERE_API_TOKEN}" "https://${CHRONOSPHERE_DOMAIN}/api/v1/config/monitors?page.token=abc123"
   ```

   The server responds with:

   ```json theme={null}
   {
     "page": {
       "next_token": "def456"
     },
     "monitors": [
       {
         "slug": "my-other-monitor",
         "name": "My Other Monitor",
         // ...
       },
       // ...
     ]
   }
   ```

3. Because the non-empty `page.next_token` response field indicates there are more
   results to fetch, request the next page:

   ```shell wrap theme={null}
   curl -H "API-Token: ${CHRONOSPHERE_API_TOKEN}" "https://${CHRONOSPHERE_DOMAIN}/api/v1/config/monitors?page.token=def456"
   ```

   The server responds with:

   ```json theme={null}
   {
     "page": {},
     "monitors": [
       {
         "slug": "yet-another-monitor",
         "name": "Yet Another Monitor",
         // ...
       },
       // ...
     ]
   }
   ```

4. When you get a response with an empty `page.next_token` field, you have reached
   the final page of results.
