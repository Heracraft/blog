---
title: Why you should hide your sanity studio hostname
description: How anyone can gain access to most sanity project data through their studio hostname
date: 2024-12-12T17:31:11+00:00
draft: true
---
Sanity Studio is a remarkable tool offering dynamic configuration and seamless editorial collaboration. But the magic of sanity means a lot of developers overlook some details in the documentation. Specifically [this little gotcha](https://www.sanity.io/docs/deployment#ed3cd78ea4eb) concerning sanity's hosted service. 

> [!info] Gotcha 
> The `sanity deploy` command works by building the source files in your studio project into static files, which are then uploaded and served from your chosen `sanity.studio` domain.
> Note that no authentication is involved when serving the built source files, so make sure not to include any sensitive data (schema files, package.json, config files, custom inputs, etc.) in your studio code. 

This design choice means that any files included in the build are publicly accessible. This is often overlooked, and all it takes is discovering a studio's hostname (e.g., `hostname.sanity.studio`). From there, one can easily access the full `schema`, `projectId` and `dataset`. Since many developers use the default public dataset (`production`), there’s often no restriction on querying the data. 
#### How this can be exploited (For Educational Purposes Only)
If you're curious about how this misconfiguration might be exploited, here's a simplified overview:

1. Find a Sanity Studio Hostname

Look for companies or projects that use sanity. One place to look is GitHub, some public repositories have the sanity studio hostname hardcoded. You can search [GitHub for "sanity.studio"](https://github.com/search?q=sanity.studio&type=code) to find studios.  Or you can head to a tech stack website and search for companies using sanity in their stack. Try using the company name as the hostname (`companyname.sanity.studio`) or some other variation, most will yield no result but some will work.

2. Inspect the Served Bundle for Config

Navigate to the studio URL and under the sources tab in chrome, search through the served bundle for a `projectId` and `dataset`
![](https://res.cloudinary.com/dpsyccfsa/image/upload/v1733545167/Sea%20Assets/h1a9szidfgxofr74tb3s.png)

3. Start Querying

Use the `projectId` and `dataset` to query data to your hearts content. A simple tool you can use run queries is [sanity client](https://sanityclient.vercel.app)--a poor imitation of the sanity vision tool-- to test out queries. I wanted a way to run queries rapidly on different projects/datasets without editing any code. At first I thought of embedding sanity studio on a Next.js project and bypass auth somehow--And I kinda did, using the `unstable_noAuthBoundary` prop, discussed [here](https://github.com/sanity-io/sanity/issues/5371)-- but both the structure and vision tool require an authorized session. So i settled for this and it works.
![](https://res.cloudinary.com/dpsyccfsa/image/upload/v1733588097/Sea%20Assets/paa4zkd6lsxeacikbjrv.png)
![](https://res.cloudinary.com/dpsyccfsa/image/upload/v1733588098/Sea%20Assets/tggoj1wp8xyg0gei3spo.png)
 

### What to do
Discovering this vulnerability was alarming. I have a couple of projects using sanity and they have the most generic `hostnames`, stuff that anyone can guess. Worse yet, the schema was in the served bundle too. So what to do?

1. Redeploy your studio on a not so generic hostname

As long as your studio hostname remains secret and you are not making queries clientside then simply making your hostname ungeuessable is a convinient fix. You go from `portfolio.sanity.studip` to `portfolio-bpe1.sanity.studio`

To update your hostname:
``` shell
sanity undeploy
```

Then redeploy with the new hostname:
```shell
sanity deploy
```

Do be warned though, your previous hostname will be available for anyone to use.

2. Self-host sanity studio

Do away with the convenience and self host sanity studio!!. Self hosting opens the door to more robust solutions and it is easier than you think!. You can use providers like netlify and vercel or your own setup. Though the former limits the number of walls you can build around your studio. Or embed the freaking thing in a Nex.js (or any other framework) under an auth protected route. 

3. Keep schema files out of your bundle

So far, I haven't found a way to do this without breaking the production build. If you discover a solution, feel free to [edit this page on GitHub](https://github.com/heracraft/blog/edit/master/content/Why%20you%20should%20hide%20your%20sanity%20studio%20hostname.md). Interestingly, the documentation suggests this is possible: *"so make sure not to include any sensitive data (schema files, package.json, config files, custom inputs, etc.) in your studio code."*  Unless I am missing something or completely reading it wrong. 