1. Use singular name for the tables. Example: "incident".
1. Use plural name for the slushbacket fields. Examples: "u_regions", "u_countries".
1. Capitalize each word of the table labels. Examples: "Catalog Task", "Support Group".
1. Capitalize only first letter of the field labels. Abbreviations can be considered as an exception. Examples: "Due date", "JWT token".
1. Use ES5 [Airbnb JavaScript Style Guide() {](https://github.com/airbnb/javascript/tree/es5-deprecated/es5) for all JavaScript code.
1. By default use full table name for GlideRecord class variables. Example: `var incident = new GlideRecord('incident')`.
1. Use ";" (semicolon) as separator if you need to show list of usernames in a string. For example, pattern "&lt;last name>, &lt;first name>" is very popular one for large enterprises because surname is more selective then a name. Example: "Zhmur, Artur; Pereiras, Michael".
1. Put old item name to sc_cat_item.meta field in case of catalog item rename. If item was used in production at least once in last 3 months. For popular items consider keeping old name in the sc_cat_item.name for some time. So that users will have time to adapt.
1. Use batch update sets whenever possible. Even if you have only 2 update sets for productionizing, go ahead and select one of them as a parent.
1. Use "- b" + Enter in the Navigator filter to quickly find and open "Scripts - Background".
