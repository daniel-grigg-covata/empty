empty
=====

## Quick Usage Guide

1. Authorize the user

```c#
var client = new CovataApiClient("YOUR-SERVER-URL", 
    new GrantPassword(APP_CLIENT_ID, 
                      APP_CLIENT_SECRET));
```

