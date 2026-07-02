---
title: "Revisiting Backstage as Developer Portal"
description: "Revisiting Backstage as a developer portal, how it evolved from a basic service catalog into a central development hub with AI agent integration, MCP actions, and self service scaffolding for modern engineering teams."
date: 2026-04-29
tags: ["dev", "ai", "platform-engineering"]
---

My first interaction with Backstage took place about three to four years ago. At that time, the feature was quite basic, basically a lightweight system for service cataloging, hosting tech docs, and providing templating engine. I admit, I didn't really utilize it properly back then. Initially, my team leveraged it solely to list our microservices, a necessary step given we managed customer data distributed across numerous clusters. We even built a custom plugin to visualize AWS billing based on customer metadata, but beyond that, our usage was limited.
In retrospect, I didn't realize that we can actually get more out of it. I missed opportunities to leverage all of its features. My focus was elsewhere, leaving the powerful potential of the platform untapped.

Last year, I had the chance to revisit Backstage, and this time, I approached it with the maturity of the platform and the richness of the modern ecosystem. The difference is astounding. Backstage has matured into a robust platform with a thriving community ecosystem. Where before we were chasing basic features, we now have dedicated plugins available for nearly every use case. The community [rbac](https://github.com/backstage/community-plugins/blob/main/workspaces/rbac/plugins/rbac-backend/README.md), [kubernetes](https://github.com/backstage/backstage/blob/master/plugins/kubernetes/README.md), [prometheus](https://github.com/RoadieHQ/roadie-backstage-plugins/tree/main/plugins/frontend/backstage-plugin-prometheus) would helps a lot if it exists back then.

Now, with the landscape changed, I want to ensure that it exists not only as a complement to developer tools but became truly central to their workflow without creating any friction.

## Backstage Through a New Lens

The service catalog remains the core engine of the portal, but its function has shifted. It is no longer a mere list, it is the singular source of truth for all our products and services. By maintaining up to date dependency and ownership data within the catalog, we gain a living, breathing diagram of the entire company's service architecture.
The most significant enhancement has been the consolidation of all project documentation within Backstage. Rather than undertaking the massive undertaking of migrating and rewriting all our existing documentation in markdown, we chose a scalable approach, integrating a [Confluence collator](https://github.com/backstage/community-plugins/blob/main/workspaces/confluence/plugins/confluence/README.md). This plugin indexes our existing Confluence knowledge base and surfaces it through Backstage's search capabilities, providing immediate value without too much disruptions.

Another feature worth to note is the scaffolder. As teams scale and require numerous services, the scaffolder grants them full autonomy to provision new services and infrastructure through self-service workflows. This drastically reduces the communication latency and friction traditionally occurs during dev to infra handoffs.

Furthermore, we now embed relevant monitoring metrics and health signals directly within each component's view. This allows both developers and managers to instantly gauge the readiness and current state of any service or product.

##  Empowering the Developer Workflow with AI

Since using coding agent is a norm now, integrating it with Backstage truly unlocks Backstage's potential, transforming it into a proactive partner in the development lifecycle.

With the integration of the [mcp actions backend](https://github.com/backstage/backstage/tree/master/plugins/mcp-actions-backend) and a dedicated [chat interface](https://github.com/backstage/community-plugins/blob/main/workspaces/mcp-chat/plugins/mcp-chat/README.md), developers can now interact with and leverage all consolidated information within Backstage through a natural language whether from inside the portal or from their own coding agent.
This trove of structured data, comprising service architecture, API details, operational ownership, and code governance guidelines is invaluable. When fed into an AI agent, this information becomes a powerful multiplier of developer productivity.

My approach for this is by exposing the Backstage MCP and integrate it to the developers' coding agent. By default, Backstage can expose catalog, scaffolder, and techdocs action. This is sufficient for normal use cases but beyond this we need to extend the action by yourselves. For example, the default MCP came with `get-entity` tools action. This can help you getting a single Backstage entity information, but how if we actually looking for all components owned by a certain team? This cases can't be covered by the standard toolings. To cover this, we developed built a [custom query action](https://backstage.io/docs/backend-system/core-services/actions-registry/) and exposed it as a tool.
With a combination of this and something like [Context7](https://context7.com), I can indirectly guide the developers in my organization to produce code that follows all best practices and actually aware of the company's internal code guidelines and standard.

This also unlocks a lot of use cases for developers. Let's say we want to build a new features that use API from other service, we don't need to switch context from our current workspace since the MCP can retrieve the relevant API docs. Or if we want to rebuild our new client facing SDK, the MCP can covers that area as well. Onboarding cost for a new team member also reduced quite a lot since all the historical data lives within the portal. In essence, the more we feed it with context, the greater the return on investment. It transforms from a static database into a dynamic, intelligent cockpit for engineering excellence.
