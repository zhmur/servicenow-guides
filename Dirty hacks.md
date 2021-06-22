# Dirty hacks

Disclaimer: never use technics described here unless absolutely necessary (achieving business goal with ANY other solution is not possible). Direct __approval__ from ServiceNow Architect is __required__ to them in production.

## GlideUser object update

There is typical business requirement to update a user's roles dynamically without the need for them to sign out and then sign back in. Typical solution using `GlideSecurityManager` will help only partially because it will not refresh GlideUser object. And as a result method `gs.getUser().hasRole('itil')` will fail (will not reflect the update). Solution is described down below. Big "Thank you" to @icerge.
```javascript
var user = gs.getUser();

// Refresh GlideSession object
var securityManager = GlideSecurityManager.get();
securityManager.setUser(user);

// Refresh GlideUser object
gs.getSession().loadUserByID(user.getID());
```
