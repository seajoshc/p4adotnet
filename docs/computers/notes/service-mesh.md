# Service mesh use cases
Mirror of [Service mesh use cases - lucperkins.dev](https://lucperkins.dev/blog/service-mesh-use-cases/)

## Service discovery
TL;DR: Access other services in the network using simple names.

Your services need to be able to automatically “find” each other via readily comprehensible names, such as service.api.production or pets/staging or cassandra. Cloud environments are highly elastic environments where many instances of a service will be gathered behind that name and hard-coding IP addresses is thus a complete non-starter.

And once a service finds another service, it needs to be able to send requests to that service without worrying that it’s going to hit a non-responsive service instance. That means that the service mesh needs to keep track of instance liveness and keep the host list as up to date as possible.

Different service meshes provide service discovery in different ways. Right now it seems most common to delegate service discovery to an external mechanism like Kubernetes DNS. At Twitter back in the day we used the Finagle naming system. Service mesh even paves the way to fully custom naming mechanisms, though I haven’t seen one that provides this.

## Encryption
TL;DR: Eliminate unencrypted traffic between services in a scalable, automated way.

It’s nice to ensure that malicious agents can’t penetrate into your internal network. Firewalls are good for that. But what happens if they manage to break in? Should they be able to run amok, snooping inter-service traffic at will? Let’s hope not. In order to prevent this scenario, you need to establish a zero-trust network in which all traffic between all of your services is encrypted. Most existing service meshes do this by providing mutual TLS (mTLS). And in some cases, mTLS works across clouds and clusters and someday probably planets.

You don’t need a service mesh for mTLS, of course. You could let each service handle its own TLS, but that would mean that you need to figure out a way to generate certs, distribute them to your service hosts, write application code that pulls in those certs from the filesystem, and then rotate those certs at established intervals. Service meshes automate mTLS using systems like SPIFFE that in turn automate certificate issuance and rotation.

## Authentication and authorization
TL;DR: Determine who is making a request and determine what they’re allowed to do before the request even reaches your service.

Services often need to know who is making a request (authentication) and, on the basis of that ascertained identity, decide what they’re allowed to do (authorization). The “who” here can be either:

Other services. This is called peer authentication. The web service wants to access the db service. Service meshes typically solve this via mTLS, whereby certificates provide the necessary ID card.

Specific human users. This is called request authentication. User haxor69 wants to purchase a new lamp. Service meshes provide various mechanisms for this, such as JSON Web Tokens.

This is something that we’re all used to doing in our application code. A request comes in, we ask our users table if the user exists and has provided the right password, then we check the permissions column in our users table, etc. With service mesh this transpires before the request even reaches your service.

Once you figure out who a request is coming from, you need to determine what they’re authorized to do. Some service meshes enable you to define simple policies about who gets to do what using YAML or the command line, while others offer integrations with generalized frameworks like Open Policy Agent. The end goal is that your services can safely assume that every request is from an allowed party and that the action is authorized.

## Load balancing
TL;DR: Distribute load across service instances in accordance with a desired pattern.

A “service” in a service mesh is very often comprised of multiple identical service instances, such as a cache service that has 5 instances today but might have 11 tomorrow. When requests are made to the cache service, those need to be distributed in accordance with some goal, such as minimizing response latency or maximizing the likelihood of reaching a healthy instance on the first try. Round-robin load balancing tends to be the most common pattern, but there are many others, such as weighted request (you can select targets to “favor”), ring hash (using consistent hashing for upstream hosts), or least requests (the instance receiving the least requests is favored).

Traditional balancers offer other features like HTTP caching and DDoS protection, but those are less widely used for east-west traffic (the standard domain for service mesh). You don’t need to use a service mesh for load balancing, of course, but service meshes do enable you to control load balancing policies on a per-service basis and from a centralized control plane, eliminating the need to run and configure dedicated load balancers in your networking stack.

## Circuit breaking
TL;DR: Stop traffic to a struggling service and control damage from worst-case scenarios.

If a service is failing to keep up with traffic for whatever reason, service meshes provide you a range of options for coping with this (a few are covered in other sections). Circuit breaking is the “nuclear option” of stopping traffic to the service. But don’t want to just stop traffic cold; you need to specify a fallback plan. That could mean applying backpressure into requesting services (make sure to configure your mesh for that!) or it could mean something like updating your status page to red and directing users to a “fail whale” page.

Service meshes enable you not just to specify both when circuit breaking should occur and what happens when it does. The “when” could mean that any number of specified maxima are exceeded: total requests in the last time period, concurrent connections, pending requests, active retries, etc.

Circuit breaking isn’t something you want to do often, but it is good to be able to have an absolute last-ditch fallback plan in place.

## Autoscaling
TL;DR: Add or remove service instances automatically based on specified criteria.

Service meshes aren’t schedulers, so they don’t perform autoscaling themselves. But they can provide the information that schedulers can use to make those decisions. Because service meshes have access to all traffic between services, they’re privy to a wealth of information about what’s going on—which services are struggling, which are so lightly trafficked that capacity is being wasted, etc.

Kubernetes, for example, can autoscale services based on CPU and memory usage of Pods, but if you want to scale based on anything else, in our case anything traffic related, you need a custom metric. A tutorial like this one shows you how to do this with Envoy, Istio, and Prometheus but it’s pretty complex. I’d love to see a scheduler/service mesh make this simpler, enabling you to specify something like “scale the auth service up if it’s exceeded a pending requests threshold for more than a minute.”

## Canary deployments
TL;DR: Roll out features or versions of a service only to a subset of users.

Let’s say you’re building a SaaS product and you have an exciting new version of your service that you want to roll out. You’ve tested it in staging and everything seems fine. But you’re not sure how it will do in the wild and don’t want to burn your credibility with users. Canary deployments are the art of exposing that feature only to some subset of users. Perhaps you want it rolled out to your most loyal users, users on the free tier, or users that have specifically opted to be guinea pigs.

Service meshes enable you to do this by specifying criteria that determine who sees which version and routing traffic appropriately. The services themselves don’t need to make that decision. Version 1 of the service can assume that all requests are from users who are supposed to see that version, and version 1.1 can assume the same thing. Meanwhile, you can slowly shift a higher and higher percentage of traffic to the new version if things are going smoothly and your guinea pigs are giving the thumbs up.

## Blue-green deployments
TL;DR: Roll out that hot new feature, but be prepared to instantly roll back.

Blue-green deployments involve releasing a new service by running your new “blue” service alongside your current “green” service. If things go smoothly and the new service is battle tested you can decommission the green service. And someday, what once was blue will sadly turn green and fade away. Blue-green deployments are unlike canary deployments because they’re designed to go out to all users; they just provide a safe harbor if something goes wrong.

Service meshes provide a highly convenient lever for pulling the plug on the blue service and instantly switching back to the safer green service. Not to mention that they provide a lot of insight that you may need in determining whether or not the blue service is ready to go.

## Health checking
TL;DR: Determine which service instances are in ship shape and respond to cases when they’re not.

Health checking is the act of making judgments about whether service instances are currently read to handle traffic. For HTTP services, for example, that might mean that you make a GET request to a /health endpoint and 200 OK means healthy and anything else means unhealthy. Service meshes enable you to specify both how health is determined and how frequently health is checked. That information can then be used for the purpose of other things, like load balancing and circuit breaking.

So health checking isn’t usually a “use case” in itself insofar and is typically used to further other goals. But you also may need to act on the results of health checks in ways that are extrinsic to other service mesh goals (e.g. updating a status page, creating a GitHub issue, or filing a JIRA ticket). And the service mesh provides a convenient mechanism for automating that.

## Load shedding
TL;DR: Re-route traffic in response to a temporary spike in usage.

If you have services that are getting clobbered with traffic, you may want to temporarily redirect, i.e. shed, some of that traffic elsewhere, potentially to a backup service or datacenter or to a persistent Pulsar topic. This enables the current service to continue to service some requests instead of falling over. Load shedding is better than circuit breaking, so you still don’t want to do it often. But it may prevent cascading failures that cripple downstream services.

## Traffic shadowing
TL;DR: Send a single request to multiple places.

Sometimes you want a given request, or some sampling of requests, to be “shadowed” to multiple services. One common example is routing a subset of production traffic to a staging service. Your main production web server makes a request to the downstream products.production service and only that service. But the service mesh intelligently copies that request and sends it to products.staging without the web server even knowing.

A subordinate use case of service mesh that could be built on top of traffic shadowing is regression testing, which is the practice of giving different versions of the same service the same requests and checking whether both services exhibit the same behavior. I haven’t yet seen a service mesh that directly integrates with a regression testing system like Diffy but it strikes me as a fruitful possibility.

## Isolation
TL;DR: Divide your mesh into mini-meshes.

Also known as segmentation, isolation is the art of dividing your service mesh into logically separate sub-networks that have no knowledge of one another. Isolation is a bit like creating virtual private networks, but with the crucial difference that you still get all the service mesh goodies, like service discovery, but with added security. If a malicious agent somehow got into a service in one sub-network, they wouldn’t be able to, for example, see which services are running in other sub-networks or snoop traffic therein.

The benefits may be organizational as well. You may want to divide services into sub-networks based on division of the company and unburden developers of the cognitive load of reasoning about the entire service mesh.

## Retries, rate limiting, and timeouts
TL;DR: Strike bread-and-butter request management tasks from your codebase.

These could all be thought of as separate use cases but I’ll smoosh them together here because what they share in common is that they’re request lifecycle tasks typically handled by application libraries. If you’re writing a (non-service-meshed) Ruby on Rails web server that makes requests to backend services via gRPC, your application code will need to determine what happens when N requests fail, you’ll need to figure out how much traffic those services can handle and hardcode that via a rate limiting library, and you’ll need to decide when to give up and let the request timeout. And if you want to update any of the above, you’ll need to stop, reconfigure, and restart.

Delegating this to the service mesh means not only that service engineers don’t need to think about them but also that they can be reasoned about in a more global way. If you’re working with a complex chain of services, e.g. A –> B –> C –> D –> E, you’ll want to reason about the entire request lifecycle. If you need to extend timeouts in service C, you want to pull an Archimedean lever to do so, not update service code and wait for the pull request to be approved and CI to redeploy the service.

## Telemetry
TL;DR: Gather all the information as you need from your services, plus some more just in case.

Telemetry is a catch-all term for metrics, distributed tracing, and logs. Service meshes provide mechanisms for handling and acting upon all three. This is where things get a bit soupy because there are so many options. For metrics you have Prometheus et al, for logs you have fluentd, Loki, and Vector et al, for distributed tracing you have Jaeger et al. Specific service meshes tend to have built-in integrations for some systems and not others. It remains to be seen if the Open Telemetry project provides some convergence.

The benefit of service mesh here is that sidecars can, in principle, collect all of the above from the services running next to them. This means that you have a single telemetry gathering system at your disposal, which enables the service mesh to handle that information in any number of ways. To give a few examples:

Tail the logs from a specific service using the CLI
Monitor request volume from the service mesh dashboard
Gather distributed tracing data and feed that into a system like Jaeger
Value judgment alert: In general, telemetry is an area where you probably don’t want your service mesh to do too much. Getting some really basic insight and checking on “golden metrics” like success rates and latency on the fly is fine, but let’s hope that we don’t see the rise of service mesh Frankenstacks that try to displace other single-purpose systems, some of which are quite good and well understood.

## Auditing
TL;DR: Those who don’t know history are condemned to repeat it.

Auditing is the art of keeping track of important events in a system. In a service mesh, that might mean knowing who has made requests to specific endpoints of specific services or how many times a particular security-related event has transpired in the last month.

Auditing is very closely related to telemetry, of course, but with the difference that telemetry is typically associated with things like performance and technical soundness, whereas auditing may have legal and other ramifications that extend outside of the strictly technical sphere (GDPR compliance, anyone?).

## Visualization
TL;DR: All hail the fancy React.js dashboard!

There may be a more specific term for this but I’m not aware of it. I just mean creating a visual representation of your service mesh or some subset thereof, and these visualizations can include things like average latency metrics, sidecar configuration information, health check results, and alerts.

Working in a service-oriented environment already bears significant cognitive load vis-à-vis working on a Majestic Monolith. Anything you can do to reduce cognitive costs is a win, and something as simple as letting developers click around on a visual representation of the mesh may provide crucial grounding (and is great for onboarding!).
