# Hubspot REST Collector IO
----
## About this Pack

This pack is built as a complete SOURCE + DESTINATION solution (identified by the IO suffix). Data collection and delivery happen entirely within the pack's context, eliminating the need to connect it to globally defined Sources and Destinations. 

This Pack is designed to collect, process, and output HubSpot audit log and login activity data via the [HubSpot Account Activity API](https://developers.hubspot.com/docs/api/settings/account-activity-api).

The Pack includes optional Splunk output processing that maps data to a Splunk-compatible format for security monitoring and compliance use cases.

## Deployment

* This pack is configured by default to use the Worker Group's *Default Destination*.
* To use the *Default Destination*: No changes are required. The pack will route the data to the destination currently set as the Default on the Worker Group.
* To use a different Destination: You must update the pack's routes to specify your desired Destination.
* For immediate functionality without requiring Pack route filter expression modifications, every bundled Source within this pack adds a hidden field: `__packsource`. This field allows for seamless routing based on the Pack source.

### Configure the Collectors

* Obtain a **Private App Access Token** from your HubSpot account.
  * Navigate to **Settings > Integrations > Private Apps** in HubSpot
  * Create a new Private App with the required scopes for audit log access (minimum of `account-info.security.read`)
* Update the `hubspot_access_token` variable with your Access Token
* Perform a **Commit & Deploy** (required for Preview to work) and then **Run > Preview** of each Collector to verify that they work correctly.
* Schedule the Collectors, adjusting the schedule as needed. Collectors requiring State Tracking already have it enabled.

**NOTE**: the `in_hubspot_login_activity` Collector will collect as much login activity as it can because the endpoint does not accept a filter. By default, it will only collect the last 2000 entries (combination of `limit` and `page limit`) and run once/day. Adjust based on your Hubspot usage activity. 

### Configure Output Format

Each data type can be configured to output data in either normalized JSON or Splunk (`_raw` + Splunk fields) format. Enable *only one* format for each pipeline.

### Configure your Destination/Update Pack Routes
To ensure proper data routing, you must make a choice: retain the current setting to use the Default Destination defined by your Worker Group, or define a new Destination directly inside this pack and adjust the pack's route accordingly.

### Commit and Deploy
Once everything is configured, perform a **Commit & Deploy** to enable data collection.

### Variables

The Pack has the following variables:
* `hubspot_access_token`: Your Hubspot Access Token
* `default_splunk_index`: Default index for the Splunk output - defaults to `hubspot`.
* `default_splunk_sourcetype`: Default sourcetype for the Splunk output - defaults to `hubspot:audit`.

## Upgrades

Upgrading certain Cribl Packs using the same Pack ID can have unintended consequences. See [Upgrading an Existing Pack](https://docs.cribl.io/stream/packs#upgrading) for details.

When upgrading, review any custom modifications made to Pipelines or Functions, as these may be overwritten.

## Release Notes

### Version 1.0.0

- Initial release:

## Contributing to the Pack
 
To contribute to this Pack, or to report any issues or enhancement requests, please connect with the maintainers on [Cribl Community Slack](https://cribl-community.slack.com).

## License

This Pack uses the following license: [`Apache 2.0`](https://github.com/criblio/appscope/blob/master/LICENSE).
