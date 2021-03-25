# Update sets naming convention

`<login>-<task>-[sequence]-[tags]`

[] - optional

Where:

login - Domain account name (Active Directory (aka Windows) account name without domain). If you have only local login ommit prefix (not "loc_zhmurart", but "zhmurart").

task - Task number which motivated you to do development. In case of Requested Item, put Catalog Task number.

sequence - Add and increase by 1 for each separate update set for the same task.

tags - Space separated words which will help you to select update set from the update set picker. Example: "NGCC import excel parse".

Update set description is mandatory. In worst case should be equal to task short description, ideally should contain brief explanation of what was developed. Consider this as a message about your development to the team. Do not copy complete story description there.
