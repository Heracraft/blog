---
title: Why you should hide your sanity studio hostname
description: How bad actors can gain access to your sanity config using a your studio hostname only.
date: 2024-12-12T17:31:11+00:00
draft: true
---
Sanity studio as a tool offers dynamic  configuration and editorial collaboration. But the magic of sanity means a lot of developers overlook some details in the documentation. Specifically [this little gotcha](https://www.sanity.io/docs/deployment#ed3cd78ea4eb). 

> [!info] Gotcha 
> The `sanity deploy` command works by building the source files in your studio project into static files, which are then uploaded and served from your chosen `sanity.studio` domain.
> Note that no authentication is involved when serving the built source files, so make sure not to include any sensitive data (schema files, package.json, config files, custom inputs, etc.) in your studio code. 

This is often overlooked, and all it takes is discovering a studio's hostname (e.g., `hostname.sanity.studio`). From there, you can easily access the full `schema`, `projectId` and `dataset`. Since many developers use the default public dataset (`production`), there’s often no restriction on querying the data.
#### Trying this out (For Educational Purposes Only)

1. Find a Sanity Studio Hostname
Look for companies or projects that use sanity. One place to look is GitHub, some public repositories have the sanity studio hostname hardcoded. You can search [GitHub for "sanity.studio"](https://github.com/search?q=sanity.studio&type=code) to find studios.  Or you can head to a tech stack website and search for companies using sanity in their stack. Try using the company name as the hostname (`companyname.sanity.studio`) or some other variation, most will yield no result but some will work.

2. Inspect the Served Bundle for Config
Navigate to the studio URL and under the sources tab in chrome, search through the served bundle for a `projectId` and `dataset`
![](https://res.cloudinary.com/dpsyccfsa/image/upload/v1733545167/Sea%20Assets/h1a9szidfgxofr74tb3s.png)

3. Start Querying
Use the `projectId` and `dataset` to query data to your hearts content. A simple tool you can use run queries is [sanity client](https://sanityclient.vercel.app)--a poor imitation of the sanity vision tool-- to test out queries.

```image-layout-a	
![](https://res.cloudinary.com/dpsyccfsa/image/upload/v1733588097/Sea%20Assets/paa4zkd6lsxeacikbjrv.png)
![](https://res.cloudinary.com/dpsyccfsa/image/upload/v1733588098/Sea%20Assets/tggoj1wp8xyg0gei3spo.png)
 ```

### Before publishing
+ Figure out hugo
+ submit an issue on cord alerting them
+ email skims and sakara
+ Add a readme to the public repo


[Buy me coffee](https://buymeacoffee.com/nehemia)