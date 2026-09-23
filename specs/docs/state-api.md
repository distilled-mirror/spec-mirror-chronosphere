> ## Documentation Index
> Fetch the complete documentation index at: https://docs.chronosphere.io/llms.txt
> Use this file to discover all available pages before exploring further.

# State API

export const ApiKeySnippet = ({title = "API Key", description = "An API key is a token that you provide when making API calls. Include the token in a header parameter called", headerName = "API-Token", exampleValue = "123"}) => {
  return <div className="bg-[#e5e7eb] dark:bg-gray-800 rounded-lg overflow-hidden">
      <div className="bg-[#D5DDEB] dark:bg-gray-700 px-6 py-4">
        <h3 className="text-gray-900 dark:text-gray-100 text-sm font-normal m-0">Security</h3>
      </div>

      <div className="bg-[#E0E6F0] dark:bg-gray-800 dark:bg-gray-750 px-6 py-4">
        <h4 className="text-gray-900 dark:text-gray-100 text-sm font-normal m-0">{title}</h4>
      </div>

      <div className="px-6 py-4">
        <div className="bg-[#EBEEF5] dark:bg-gray-900 p-6 rounded border border-gray-300 dark:border-gray-700">
          <h5 className="text-gray-900 dark:text-gray-100 text-sm font-normal mb-4 mt-0">Chronosphere API token</h5>

          <p className="text-gray-800 dark:text-gray-300 text-sm mb-3 leading-relaxed">
            {description}{" "}
            <code className="bg-gray-100 dark:bg-gray-800 text-gray-900 dark:text-gray-100 px-2 py-1 rounded text-xs font-mono border border-gray-300 dark:border-gray-700">
              {headerName}
            </code>
            .
          </p>

          <div className="mt-4">
            <span className="text-gray-800 dark:text-gray-300 text-sm">Example: </span>
            <code className="bg-gray-100 dark:bg-gray-800 text-gray-900 dark:text-gray-100 px-2 py-1 rounded text-xs font-mono border border-gray-300 dark:border-gray-700">
              {headerName}: {exampleValue}
            </code>
          </div>
        </div>
      </div>
    </div>;
};

<Badge color="green" size="lg" icon="cog">v1</Badge>

The State API provides HTTP/JSON REST endpoints for accessing the runtime state of
the Chronosphere system.

Use this link to download the raw Swagger specification:
[Download](/openapi/api_v1_state_swagger.json)

<ApiKeySnippet />


## Related topics

- [Install the Chronosphere Terraform provider](/tooling/infrastructure/terraform/install.md)
- [ListCloudIntegrations](/tooling/api-info/definition/operations/ListCloudIntegrations.md)
- [CreateCloudIntegration](/tooling/api-info/definition/operations/CreateCloudIntegration.md)
- [Discover and scrape Kubernetes resources with Chronosphere Collector](/ingest/metrics-traces/collector/discover/monitor-kubernetes.md)
- [UpdateCloudIntegration](/tooling/api-info/definition/operations/UpdateCloudIntegration.md)
