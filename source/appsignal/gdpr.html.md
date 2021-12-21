---
title: "General Data Protection Regulation (GDPR)"
---

AppSignal has always had a strong focus on protecting any data we collect and process. As a European owned and operated business we appreciate the solidification of this through the General Data Protection Regulation (GDPR) laws that enter into effect on May 25, 2018.

Below you'll read how GDPR applies to AppSignal, our customers and the people our customers collect data from.

## Compliance in relation to our employees and vendors

AppSignal B.V. is the entity that owns and operates the AppSignal service, a business located in the Netherlands and therefore bound by Dutch and European law. This entity is fully GDPR compliant, which means we only request and process data based on legal bases as defined in the GDPR. In cooperation with our counsel we have made a thorough assessment of all of our processes and data stores.

Where needed, we changed our internal policies and procedures to be in compliance with GDPR, and deleted data that we didn't need or want. We also defined a new Privacy Policy and wrote a Data Processing Agreement (more on that below).

Finally, we reached out to our vendors to request agreements to ensure that we remain compliant when using their services.

## Compliance in relation to our customers

In relation to our customers, AppSignal is both a Data Controller and a Data Processor depending on the type of data collected.

### Data Controller

AppSignal is the Data Controller for the information we collect about our customers and visitors, which means that we determine the "purposes and means" of the data we collect as the Controller. Some examples: their name, their email address, their credit card number, and any other data that we collect based on the GDPR legal bases. This data is safeguarded by various policies and procedures.

When sharing data with vendors, we have made sure there are contracts in place that ensure they also receive and process this data in a lawful way.

You can read more about the data we collect for which purpose in our new [Privacy Policy](https://www.appsignal.com/privacy-policy).

### Data Processor

Our customers are the Data Controllers for the data that their applications gather and send to AppSignal. AppSignal processes that data on behalf of them, which makes us the Data Processor. To enable our customers to be fully GDPR compliant while using AppSignal, we have taken various measures.

In short: AppSignal doesn't wish to receive any personal data about your visitors, and will provide the tools that enable you to strip this information before sending it to AppSignal for processing. For more details, see below.

#### Data Processing Agreement

We have created a Data Processing Agreement that explains eg. how we are a Data Processor, how we limit processing to the level needed to be able to provide the AppSignal service, that we will ensure that our vendors are compliant too, etc (this is an incomplete list for illustrative purposes only; please see the full Data Processing Agreement for all the details).

The Data Processing Agreement is accessible to all owners for an organization on AppSignal. Open the Data Processing Agreement for [your organization](https://appsignal.com/redirect-to/organization?to=admin/data_processing_agreement) (there is one for each organization). You can digitally sign the Data Processing Agreement, and once signed we will display who signed it and when.

#### Allowed request headers only

We have always made sure to strip any personal data from incoming events such as database queries, but we did store request headers such as `REMOTE_ADDR`. Depending on architecture, this can expose useful information about load balancers or internal IP addresses. In most cases though, this sent visitor IP addresses to AppSignal. Since IP addresses are personal data, we have created a default allowlist of headers that we believe will not contain personal data. Customers can add additional headers to the allowlist ([Ruby](/ruby/configuration/options.html#option-request_headers) / [Elixir](/elixir/configuration/options.html#option-request_headers)) as needed, as long a they don't identify an individual (eg. if `REMOTE_ADDR` contains the IP address of a load balancer, that's fine).

Any non-allowlisted headers are stripped before being sent to AppSignal.

#### Filtering options for parameters and session data

We've had filtering options for _parameters_ for a long time already, but in our [parameter filtering documentation](/application/parameter-filtering.html) we now stress the importance with regard to GDPR.

Recent Ruby gem and Elixir package releases expand this filtering feature to [session data](/application/session-data-filtering.html) too. It allows customers to send parameters to AppSignal without exposing personal data, by replacing that data with `[FILTERED]`. Alternatively, a customer can choose to not send any parameters ([Ruby](/ruby/configuration/options.html#option-send_params)) or session data ([Ruby](/ruby/configuration/options.html#option-skip_session_data) / [Elixir](/elixir/configuration/options.html#option-skip_session_data)) at all.

Data is filtered before being sent to AppSignal.

#### Requirements for Tagging

We have changed the documentation for our [Tagging feature](/application/tagging.html) to strongly state that no personal data should be sent to AppSignal in tags, and that user IDs, hashes or pseudonymized identifiers should be used instead.

#### Data removal procedure

Any details about a request are stored within what we call "samples". These samples have a retention of 30, 45 or 60 days depending on plan. This has always been the case, but is important with regard to GDPR as well. It means that if any personal data was collected accidentally, it will still be purged after a maximum of 60 days. After that, we only keep aggregated data.

If a customer notices that personal data was accidentally sent as part of a sample, we have instated a procedure that allows us to remove one or more samples when requested in an email to support@appsignal.com.
