# Pack Name
Cribl HubSpot Rest Collector

## About this Pack

This Pack is designed to collect, process, and output HubSpot audit log data via the [HubSpot Account Activity API](https://developers.hubspot.com/docs/api/settings/account-activity-api).

The Pack includes Splunk output processing that maps data to a Splunk-compatible format for security monitoring and compliance use cases.

## Deployment

After installing the Pack, you must perform the following:

### Add HubSpot Credentials to Cribl Stream
*Note*: If you use an external secret store, the authentication function will have to be modified to reference those secrets using the proper names.

* Obtain a **Private App Access Token** from your HubSpot account.
  * Navigate to **Settings > Integrations > Private Apps** in HubSpot
  * Create a new Private App with the required scopes for audit log access
* Open the worker group that will contain the HubSpot collector.
* Navigate to **Worker Group Settings > Security > Secrets**
* Create 1 Secret:
   * Name: `hubspot-access-token`, of secret type **text**, containing the **Private App Access Token**.

### Update and Enable the Collector

* Navigate to **Sources > Collectors > REST** within the HubSpot pack. There is 1 collector available:
   * in_hubspot_audit: This API collects [Account Activity / Audit Logs](https://developers.hubspot.com/docs/api/settings/account-activity-api) including login events, security events, and user activity.

* The base API URL is pre-configured as `https://api.hubapi.com`.
* Perform a Run > Preview to verify that the Collector works correctly.
* To schedule data ingestion, click the **Schedule** button under the Actions column of the Collector and toggle the **Enabled** button to enable data flow. Modify the cron schedule if required; the default is set to run the Collector every 5 minutes. Confirm the **State tracking** parameter is enabled.

### Required HubSpot Scopes

Ensure your Private App has the following scope enabled:

| Collector | Required Scopes |
|-----------|----------------|
| in_hubspot_audit | `account-info.security.read` |

### Outputs

### Update and Validate the Splunk Destination

* Create a [HEC token in Splunk](https://help.splunk.com/en/splunk-enterprise/get-started/get-data-in/10.0/get-data-with-http-event-collector/set-up-and-use-http-event-collector-in-splunk-web).
* Populate the **Splunk HEC Endpoints** parameter with the Splunk HEC URL.
* Populate the **HEC Auth token** with the token created in Splunk.

## Pack Configurable Items 
The following are the in-Pack configurable items - review/update them as needed. 

### Variables

The Pack has the following variables:
* `default_splunk_index`: Default index for the Splunk output - defaults to `hubspot`.
* `default_splunk_source`: Default source for the Splunk output - defaults to `hubspot:api`.
* `default_splunk_sourcetype`: Default sourcetype for the Splunk output - defaults to `hubspot:audit`.

## Upgrades

Upgrading certain Cribl Packs using the same Pack ID can have unintended consequences. See [Upgrading an Existing Pack](https://docs.cribl.io/stream/packs#upgrading) for details.

When upgrading, review any custom modifications made to Pipelines or Functions, as these may be overwritten.

## Release Notes

### Version 0.0.1 - 2026-01-21

Initial release:
* Added collector for HubSpot Account Activity / Audit Logs
* Splunk HEC output with field mappings
* State tracking for incremental data collection

## Contributing to the Pack
 
To contribute to this Pack, or to report any issues or enhancement requests, please connect with the maintainers on [Cribl Community Slack](https://cribl-community.slack.com).

## License

This Pack uses the following license: [`Apache 2.0`](https://github.com/criblio/appscope/blob/master/LICENSE).
