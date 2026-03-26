The documentation structure should be as follows:

- Overview. Explains the purpose of the application, its major features, and the dashboard.
- Configuration.
   - Specifying services to monitor.
   - Choosing thresholds for reporting.
   - Selecting reporting services (emails, Slack, texts, text logging, and so forth).
   - Integration instructions (for Slack and other application).
- API reference. CLI reference for API integration into custom applications and in user-defined scripts.
   - OAD (Open API Description) document that shows all endpoints and methods, path parameters, query parameters, sample responses, response codes, and so forth.
   - Instructions for getting credentials and for authenticating.
   - Quick start to send your first "Hello world" call.
   - Tutorials for specific, common use cases.
   - Information on rate limits.

   This documentation covers the vast majority of the usability of your application. I've combined service configuration, alerting rules, and integrations into one section, since they logically fit together. Also, I think that it's easier for comprehension to integrate the dashboard into the introduction.