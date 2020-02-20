1. Put old item name to sc_cat_item.meta field in case of catalog item rename. If item was used in production at least once in last 3 months. For popular items consider keeping old name in the sc_cat_item.name for some time. So that users will have time to adapt.
2. Use ";" (semicolon) as a separator if you need to show (in a field message) a couple of usernames in a string. For example, pattern "<last name>, <first name>" is very popular one for large enterprises because surname is more selective then a name. Example: "Zhmur, Artur; Pereiras, Michael".
3. Use ES5 [Airbnb JavaScript Style Guide() {](https://github.com/airbnb/javascript/tree/es5-deprecated/es5) for all JavaScript code.
4. By default use full table name for GlideRecord class variables. Example: `var incident = new GlideRecord("incident")`.
5. Do not use plural for ServiceNow table names. Example: `incident`.
