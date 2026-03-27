# Project Sentinel

## Documentation structure

The documentation structure should be as follows:

- Overview. Explains the purpose of the application, its major features, and the dashboard.
- Configuration.
   - Specifying services to monitor.
   - Choosing thresholds for reporting.
   - Selecting reporting services (emails, Slack, texts, text logging, and so forth).
   - Integration instructions (for Slack, Jira, and other applications).
- API reference. CLI reference for API integration into custom applications and into scripts.
   - OAD (Open API Description) document that shows all endpoints and methods, path parameters, query parameters, sample responses, response codes, and so forth.
   - Instructions for getting credentials and for authenticating.
   - Quick start to send your first "Hello world" call.
   - Tutorials for specific, common use cases.
   - Information on rate limits.

   This documentation approach I have outlined covers the vast majority of the usability of your application. I have combined service configuration, alerting rules, and integrations into one section, since they logically fit together. Also, I think that it is easier for comprehension to integrate the dashboard into the introduction.

## User workflow

   With regard to documenting user workflows, there are too many to document end-to-end, since you have numerous services to select from, different thresholds to set, and various alerts/integrations to choose from. If there were only five possibilities to choose from in each category, there would be 125 different combinations to document. Instead of choosing a workflow and describing it end-to-end, I would focus on each section of the configuration documentation being clear and complete. I would also inform the user at the beginning of the configuration documentation section of the configuration flow: choose a service to monitor → select a threshold for alerts → select reporting options → configure reporting integrations.

## Anticipating challenges

One common challenge a developer might face is authentication. Because these are microservices, the monitoring service may need several different credentials for different cloud and on-prem environments. This is a common problem, so I would allocate extra space in the documentation for authentication.

Another common challenge would be developers wanting to use an SDK instead of a raw API call. I know that this is a common desire because popular platforms like GitHub develop SDKs precisely for this reason. To address this challenge, I would write tutorials for common sentinel API use cases, using the languages typically used by developers who access Sentinel (in addition to `curl`).


   