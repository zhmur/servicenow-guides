1. Put old item name to sc_cat_item.meta field in case of catalog item rename. If item was used in production at least once in last 3 months. For popular items consider keeping old name in the sc_cat_item.name for some time. So that users will have time to adapt.
2. Use ";" (semicolon) as a separator if you need to show list of usernames in a string. For example, pattern "&lt;last name>, &lt;first name>" is very popular one for large enterprises because surname is more selective then a name. Example: "Zhmur, Artur; Pereiras, Michael".
3. Use ES5 [Airbnb JavaScript Style Guide() {](https://github.com/airbnb/javascript/tree/es5-deprecated/es5) for all JavaScript code.
4. By default use full table name for GlideRecord class variables. Example: `var incident = new GlideRecord('incident')`.
5. Use singular names for tables. Example: "incident".
6. Use batch update sets whenever possible. Even if you have only 2 update sets for productionizing, go ahead and select one of them as a parent.
7. Capitalize only first letter of the field label. Abbreviations can be considered as an exception. Examples: "Due date", "JWT token".
8. Capitalize each word of the table label. Examples: "Catalog Task", "Support Group".
9. Use plural for slushbacket fields.
