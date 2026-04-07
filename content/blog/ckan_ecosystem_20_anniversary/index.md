---
title: "The CKAN ecosystem — what it felt like when extensions started outpacing the core"
date: 2026-04-06
draft: false
tags: ["ckan"]
categories: ["Development", "CKAN"]
showComments: true
---

<style>
    .ckan-repo-badge {
        padding: 5rem
    }

    @media (max-width: 970px) {
        .ckan-repo-badge {
            padding: 1rem
        }
    }
</style>

# The CKAN Ecosystem at 20: Extensions, Evolution, and the Long Tail

<div class="ckan-repo-badge">
    {{< github repo="ckan/ckan" showThumbnail=true >}}
</div>

Can you imagine—version 0.1 of CKAN was released all the way back in 2006. And who could have imagined that such a long journey lay ahead. I'll admit, I wasn't around to witness CKAN in those days—I was still in school. Fortunately, the topic of this article, namely the CKAN ecosystem, allows us to jump a bit forward in time.

It wasn't until 2010 that the idea of a plugin system emerged, which, from today's perspective, was clearly the right decision. If GitHub is to be believed, there are more than 2.3K public repositories starting with `ckanext-`. And that's only the public ones—there is also a common practice of maintaining private repositories that contain the business logic of data portals.

Extensions serve several critical roles. First, they are the canonical entry point for building a custom data portal. Anyone using CKAN can easily create their own extension to encapsulate changes that make their portal unique. Second, they extend CKAN's capabilities in many directions. They provide a platform for bold experimentation. The pace of extension development consistently outstrips that of CKAN core, as they operate at different scales and typically involve a much narrower decision-making process. This makes it possible to build things that may never make it into core—or will only do so much later.

But it all started quite modestly. Did you know that the first extension in CKAN core was `stats`?

## **Early Ecosystem Formation and OKFN Origins**

CKAN wasn't plugin-ready from the start. In its earliest years, the system was a monolithic application — extensibility wasn't part of the architecture. That changed around 2010, when the initial plugin system was implemented, introducing the ability for external modules to hook into core behavior rather than modifying it directly. It was a pivotal moment. What had been a single codebase now had a mechanism for growing outward.

This work happened under the umbrella of the Open Knowledge Foundation (OKFN), where CKAN originated under the leadership of Rufus Pollock. Once the plugin architecture was in place, experimentation followed quickly. Extensions such as `ckanext-deliverance` (2010) and early versions of `ckanext-disqus` (2010) show that the community wasted no time exploring the separation between core functionality and pluggable components.

## From Core Feature to Extension (and Back Again)

The first steps toward a plugin system appeared around 2010, but it was CKAN 2.0 that brought full plugin support — a major architectural overhaul that made extensibility a first-class concern rather than an afterthought. The bet was that the community would build things the core team hadn't thought of, and over the following decade, that bet clearly paid off.

Even before 2.0, though, the boundary between core and extension was already fluid. A notable example is `ckanext-stats`, which began as a standalone extension and was later incorporated into core functionality. This may have been one of the earliest cases where an extension effectively "graduated" into the core system, setting a precedent that continues to this day.

Other early components show similar ambiguity. Extensions such as `ckanext-apps` and `ckanext-follower` appear in early OKFN repositories and documentation, with some features later becoming part of core functionality (or disappearing entirely). Even in CKAN 1.3.3b documentation, extensions like Disqus integration, search components, asynchronous queues, and custom forms are explicitly listed as part of the system's extension model. Over time, several of these capabilities either migrated into core or evolved into separate, more specialized implementations.

This period reflects an experimental architecture: features were moved back and forth between core and extension space as their importance, stability, and generality became clearer.

With CKAN 2.0 (2013), the system shipped with a set of tightly integrated "core extensions," including components such as the DataStore, multilingual support, and multiple resource preview tools. These preview components — covering formats like PDF, JSON, and tabular data — were not static either. Over time, they were replaced or refactored into separate extensions such as `ckanext-pdfview` or newer preview plugins, reflecting ongoing decomposition of core functionality into more maintainable units.

A similar pattern can be observed in data ingestion and harvesting. Early versions of harvesting functionality were initially embedded in core or tightly coupled code paths, before being extracted into `ckanext-harvest` in 2011. The initial commit explicitly describes the goal of consolidating scattered harvesting logic from core and other extensions into a single, dedicated module. This is a clear example of the reverse movement: core → extension.

By the early 2010s, extensions such as `ckanext-googleanalytics`, `ckanext-qa`, and `ckanext-harvest` were already active and widely used. Many of these remain in use today, indicating that some early architectural decisions produced long-lived components.

The scale of experimentation is also visible in domain-specific deployments. For example, `ckanext-dgu` (used in data.gov.uk) dates back to 2010 and contains a broad set of customizations: from dataset forms and harvesting integration to Drupal-based authentication and reporting pipelines. Such extensions were not merely plugins; they were often full application layers built on top of CKAN, and they influenced how the core platform evolved.

## Extension Evolution and Deprecation

A recurring pattern in the CKAN ecosystem is that extensions do not remain static. They evolve, overlap, and are eventually replaced as better approaches emerge. This is not an exception but a structural characteristic of a community that continuously experiments with improving usability and architecture.

A clear example of this lifecycle can be seen in dataset preview tools. In earlier versions such as CKAN 1.8, tabular previews were handled via the `reclineview` extension, which originally existed as a separate component for rendering and interacting with structured data. It represented an early attempt to provide in-browser exploration of tabular resources.

Later, in CKAN 2.7.0 (released 2017-08-02), the `datatablesview` extension was introduced as a more modern alternative. For a period, both `reclineview` and `datatablesview` coexisted, reflecting a typical transition phase where legacy functionality is gradually phased out rather than removed abruptly. This dual support continued until CKAN 2.11 (2023), when `reclineview` was finally removed from the core extension set, consolidating the approach around `datatablesview`.

This kind of replacement cycle is not accidental but reflects a broader ecosystem dynamic: extensions act as evolutionary units where experimentation happens first, adoption follows, and eventual consolidation occurs when a clearer winner emerges.

In that same spirit, newer work such as `ckanext-tables` continues this trajectory. It explores new approaches to tabular data presentation and may, over time, either replace or reshape how `datatablesview` is used. Whether it becomes a successor or simply another specialized option is less important than the underlying pattern: the ecosystem continuously iterates through overlapping implementations until a more effective abstraction stabilizes.

{{< mermaid >}}
graph LR
    subgraph "Tabular data preview"
        B1["reclineview (~2012)"] --> B2["datatablesview (2017)"]
        B2 --> B3["tables (2024)"]
    end

    style B1 fill:#d3d1c7,stroke:#5f5e5a,color:#2c2c2a
    style B2 fill:#9fe1cb,stroke:#0f6e56,color:#04342c
    style B3 fill:#cecbf6,stroke:#534ab7,color:#26215c
{{< /mermaid >}}

## Evolving Data Import Mechanisms

Another area where this evolution is particularly visible is data ingestion into the DataStore. Over time, CKAN has moved from a single, core-driven approach toward a more diverse ecosystem of tools, each reflecting different priorities.

Back in 2014, with the release of version 2.2, CKAN introduced DataPusher—a built-in service for automatically importing tabular data into the DataStore. It replaced the earlier `ckanext-datastorer` and marked a significant improvement in robustness, ease of deployment, and user experience, including a dedicated interface for re-importing data and inspecting logs. At the time, this was a clear step toward making data ingestion a first-class, out-of-the-box capability.

As the ecosystem matured, however, performance and scalability demands led to new approaches. The emergence of `ckanext-xloader` in 2017 reflected this shift. Designed as a replacement for DataPusher, it prioritized speed and reliability, offering dramatically faster imports for large datasets while remaining relatively simple in scope.

More recently, `datapusher-plus` has taken the idea further by combining the strengths of both predecessors. It retains the performance characteristics of XLoader while reintroducing and extending DataPusher's schema inference capabilities. In addition, it enables more advanced workflows, such as suggesting metadata through the formulas defined in the dataset schema.

{{< mermaid >}}
graph LR
    subgraph "Data ingestion"
        A1["datastorer (~2012)"] --> A2["DataPusher (2014)"]
        A2 --> A3["xloader (2017)"]
        A3 --> A4["datapusher-plus (2021)"]
    end

    style A1 fill:#d3d1c7,stroke:#5f5e5a,color:#2c2c2a
    style A2 fill:#9fe1cb,stroke:#0f6e56,color:#04342c
    style A3 fill:#9fe1cb,stroke:#0f6e56,color:#04342c
    style A4 fill:#cecbf6,stroke:#534ab7,color:#26215c
{{< /mermaid >}}

Seen in the broader context of CKAN's evolution, these tools illustrate a recurring pattern: core functionality establishes a baseline, but innovation increasingly happens in extensions. Data ingestion, once a single built-in mechanism, has become a space where multiple solutions coexist—each pushing the boundaries of performance, flexibility, and intelligence in different ways.

## Balancing Core and Modularity

There has always been a design tension within the CKAN ecosystem: deciding what belongs in core versus what should live as an extension.

On one side, there is a pragmatic argument for promoting certain extensions into the core. Some features—like file uploads—are considered foundational. If a capability is both critical to most deployments and serves as a building block for further functionality, keeping it as an optional extension creates friction. Other extensions cannot reliably depend on it, which limits composability. In that sense, moving something like `ckanext-files` into CKAN core—planned for release in version 2.12—is less about convenience and more about establishing a guaranteed baseline. It ensures that downstream features can safely assume its presence and build on top of it without defensive checks or fallback logic.

There is also an evolutionary aspect. Many extensions begin as independent modules with a narrow decision-making scope, which allows them to iterate faster and mature without the constraints of core governance. Once they prove their value and stability, merging them into the core becomes a logical step to ensure long-term maintenance and wider adoption.

On the other side, there is a strong architectural preference for modularity. A minimal core with well-defined interfaces offers flexibility: different implementations of file storage, permissions, search engines, or even databases can be swapped in depending on context. This approach aligns with classic pluggable architecture principles, where the core acts as an orchestration layer rather than a feature provider. It also improves testability and portability—for example, enabling lightweight environments like SQLite for testing, even if production relies on PostgreSQL-specific features such as JSONB.

The handling of activity streams and tracking illustrates the opposite decision. These features were originally embedded in core but behaved more like side effects than integral components. They were loosely coupled, easy to forget during implementation, and not essential for system correctness. By extracting them into extensions and relying on signals/hooks, the system retained the same behavior while improving separation of concerns. Additionally, this made the mechanism more transparent to third-party developers, who could now see explicit patterns for extending actions with activity logging.

In summary, the implicit guideline that emerges is not ideological but functional: core should contain only what is both essential and foundational for other features, while everything else—especially side effects or replaceable components—should remain modular. The challenge is that "essential" is contextual, and each decision shifts the balance between stability, flexibility, and architectural purity.

## The Long Tail of CKAN Extensions

A useful way to understand the CKAN ecosystem today is to look at its extension landscape not as a static registry, but as a long, uneven tail of experimentation. A simple search for repositories starting with `ckanext-` returns thousands of results, many of them inactive for years. Sorting by last update quickly reveals a layer of abandoned or frozen ideas, alongside a smaller subset of actively maintained components. The ecosystem is large, but only partially alive at any given moment.

At the same time, the official extension registry at extensions.ckan.org lists only a few hundred entries. The discrepancy between "existing code" and "curated extensions" highlights an important property of the ecosystem: visibility and adoption are not the same thing as existence. Many extensions persist as historical artifacts, even if they no longer participate in current CKAN evolution.

## The Most Widely Adopted CKAN Extensions

Across the CKAN ecosystem, a relatively small group of extensions has become foundational in real-world deployments. While thousands of `ckanext-*` repositories exist, only a subset consistently appears across production instances, shaping how CKAN is actually used.

Interoperability is one of the most critical areas. `ckanext-dcat` enables DCAT-based metadata exchange and is essential for data portal federation. `ckanext-harvest` complements this by providing the harvesting infrastructure used to ingest datasets from external catalogs. In domains where geospatial data is central, `ckanext-spatial` extends CKAN with spatial capabilities, making it a standard component in many government deployments.

Schema management is largely defined by `ckanext-scheming`, which allows instances to move beyond CKAN's default metadata model and define structured, domain-specific schemas.

Rather than relying solely on UI-level translations, `ckanext-fluent` introduces true multi-language field support at the data level, allowing datasets, resources, organizations, and groups to store values in multiple languages simultaneously. It integrates tightly with scheming, providing presets such as `fluent_text` and `fluent_tags` that expand a single logical field into multiple language-specific inputs and store them as structured objects in the API.

In practice, this pairing—scheming for structure and fluent for multilinguality—has become a common pattern in international CKAN deployments, especially in government portals where metadata must be available in multiple languages.

For data ingestion, the ecosystem has evolved through several generations. DataPusher (introduced into core in CKAN 2.2) established the baseline, while `ckanext-xloader` improved performance and robustness. More recently, `datapusher-plus` combines performance with advanced schema and metadata inference, reflecting increasing demands for intelligent data processing.

Content management within CKAN portals has traditionally relied on `ckanext-pages`, which provides a simple way to manage static pages alongside datasets.

Analytics and monitoring are commonly implemented via `ckanext-googleanalytics` or built-in tracking, while more advanced auditing use cases are addressed by `ckanext-event-audit`, which captures system-level events and API activity.

Finally, dataset preview functionality has historically been distributed across multiple extensions. Tools such as `ckanext-pdfview`, `ckanext-officedocs`, `ckanext-zippreview`, and others handle specific file formats, forming the backbone of in-browser data inspection.

For discovering and exploring this ecosystem, curated directories such as CKAN Ecosystem Catalog provide a more structured view compared to the raw volume of repositories on GitHub.

## Emerging Alternatives and the Next Generation of Extensions

Alongside these established tools, a new wave of extensions is actively reshaping parts of the CKAN ecosystem. These are not always direct replacements, but they revisit earlier design decisions with modern tooling, improved UX, or clearer abstractions.

In content management, `ckanext-blocksmith` (2025–present) represents a shift away from simple static page editing toward more flexible, component-based layouts, effectively bringing lightweight CMS capabilities into CKAN. In a similar space, `ckanext-content` comes to replace legacy extensions like `ckanext-pages` and `ckanext-showcase` with a schema-driven, configurable solution that supports custom content types, translations, file uploads, templating, and URL aliasing.

{{< mermaid >}}
graph LR
    subgraph "Content management"
        C1["pages (~2014)"] --> C2["blocksmith (2025)"]
        C1 --> C3["content (2025)"]
    end

    style C1 fill:#d3d1c7,stroke:#5f5e5a,color:#2c2c2a
    style C2 fill:#cecbf6,stroke:#534ab7,color:#26215c
    style C3 fill:#cecbf6,stroke:#534ab7,color:#26215c
{{< /mermaid >}}

Visualization is also being revisited. `ckanext-charts` (2024–present) builds on earlier efforts like `basiccharts` but introduces a more modern and maintainable approach to chart rendering and integration with datasets.

Preview and data exploration tools continue to evolve as well — and this matters more than it might seem at first glance. For many users, the resource preview is the data portal experience. Not everyone downloads a CSV to open in a spreadsheet; often, a quick look at what's inside a file is all that's needed to decide whether a dataset is relevant. Good previews lower the barrier to actually using open data, which is arguably the whole point.

While traditional extensions like `ckanext-zippreview` remain widely used, newer tools such as `ckanext-unfold` focus specifically on improving the experience of browsing archive contents. In parallel, structured data inspection is being enhanced by extensions like `ckanext-pygments`, `ckanext-json-viewer`, and `ckanext-tables`, which provide richer, more interactive ways to explore datasets directly in the browser. The direction is clear: move from "here's a download link" toward "here's the data, explore it right now."

{{< divider >}}

One of the more fundamental challenges in the CKAN ecosystem has always been theming. CKAN's templates have historically been tightly coupled to Bootstrap — class names are hardcoded throughout the markup, which means that changing the underlying CSS framework (or even upgrading Bootstrap itself) requires touching templates across core and every extension. For a platform that prides itself on extensibility, the UI layer has remained surprisingly rigid. This is another area where extensions are now pushing boundaries that core hasn't addressed.

`ckanext-theming` takes an interesting approach to this problem. Instead of templates being hardcoded with framework-specific classes, it introduces a macro-based abstraction layer — templates call semantic helpers like `ui.button()` or `ui.card()`, and the active theme decides how those actually render. Bootstrap 5, Tailwind, Bulma, Pico CSS — in theory, any of them can serve as the backend for the same set of templates.

But this isn't just about swapping CSS frameworks. The deeper value is stability. Today, when CKAN upgrades its core theme or changes its markup, community themes tend to break — especially the ones that haven't been updated in years. Under the macro-based approach, themes that target the abstraction layer rather than raw Bootstrap classes would remain compatible across CKAN upgrades, even if they haven't been touched in 5–8 years. And when something does change in the abstraction contract, `ckanext-theming` includes built-in tooling to detect and report incompatibilities, making the breakage visible and manageable rather than silent.

{{< mermaid >}}
graph TB
    subgraph Templates
        A["Jinja Templates<br/>(dataset, group, etc.)"]
    end

    subgraph "UI Macros Layer"
        B["ui.button()"]
        C["ui.card()"]
        D["ui.alert()"]
        E["ui.nav()"]
    end

    subgraph Theme
        F["Theme Engine"]
    end

    subgraph CSS Frameworks
        G["Bootstrap 5"]
        H["Tailwind"]
        I["Bulma"]
        J["Pico CSS"]
    end

    A -->|uses| B
    A -->|uses| C
    A -->|uses| D
    A -->|uses| E

    B -->|delegates| F
    C -->|delegates| F
    D -->|delegates| F
    E -->|delegates| F

    F -->|"ckan.ui.theme = bootstrap"| G
    F -->|"ckan.ui.theme = tailwind"| H
    F -->|"ckan.ui.theme = bulma"| I
    F -->|"ckan.ui.theme = bare"| J

    style A fill:#e1f5ff
    style F fill:#fff3cd
    style G fill:#d4edda
    style H fill:#d4edda
    style I fill:#d4edda
    style J fill:#d4edda
{{< /mermaid >}}

The extension is still in early days (version 0.0.1, CKAN 2.12 only), but the codebase is well-structured with proper testing, type checking, and modern Python tooling. It addresses a real pain point for anyone who's tried to maintain a custom CKAN theme across multiple upgrades.

{{< divider >}}

Operational tooling is another area where new extensions are emerging. `ckanext-selfinfo` provides a consolidated, in-UI diagnostics and monitoring layer for CKAN instances, exposing system metrics, configuration state, installed extensions, and runtime errors directly to administrators. It reduces reliance on external logs by centralizing operational visibility within the platform itself.

In a similar direction, `ckanext-admin-panel` represents a broader attempt to rethink CKAN's administrative experience. Rather than exposing scattered configuration screens and CLI-driven workflows, it introduces a more cohesive administrative interface that brings together user management, system monitoring, extension management, and log inspection into a single UI. The extension is positioned as a "next-generation admin interface," aiming to make routine operational tasks more accessible and centralized. It also introduces additional capabilities like runtime configuration editing, effectively turning the admin panel into an extensible control layer for CKAN itself.

These newer extensions illustrate a continuation of a long-standing pattern: established solutions provide stability and widespread adoption, while newer ones experiment with improved performance, usability, and developer experience. Over time, some of these approaches may replace existing tools, while others will coexist, further expanding the diversity of the CKAN ecosystem.

{{< carousel images="gallery/*" captions="{parquet.png:The [parquet extension](https://github.com/DataShades/ckanext-parquet), self-info.png:The [selfinfo extension](https://github.com/DataShades/ckanext-selfinfo), pygments.png:The [pygments extension](https://github.com/DataShades/ckanext-pygments), unfold.png:The [unfold extension](https://github.com/DataShades/ckanext-unfold), json-viewer.png:The [json-viewer extension](https://github.com/DataShades/ckanext-json-viewer), block-smith.png:The [blocksmith extension](https://github.com/DataShades/ckanext-blocksmith), tables.png:The [tables extension](https://github.com/DataShades/ckanext-tables)}" interval="2500">}}

<br>

## What's Next?

By 2026, the GitHub search for `ckanext-` yields over **2,300 public repositories**. This is a staggering number. It represents tens of thousands of hours of donated time from developers across the globe.

I began my own active journey in this ecosystem about 7 years ago (circa 2019) when I joined **Link Digital**. My first extension-related contributions were `audioview` and `videoview` for the core—fairly simple features that utilize the HTML5 `<audio>` and `<video>` tags. Since then, I've had my hands on over 70 public extensions—creating over 30 from scratch.

This work is not without its challenges. The "well-being" of an extension depends entirely on developer activity. We have seen "abandonware" extensions that were vital in 2014 but failed to make the jump to Python 3 or CKAN 2.9.

Looking ahead, there are a few things I think about. The extension ecosystem will keep growing—that much is certain. But the real question is whether it grows in a healthy way. We already have over 2,300 repositories, and many of them are effectively dead. The challenge isn't creating more extensions; it's maintaining the ones that matter and making them discoverable. Better tooling around extension quality, compatibility tracking, and curation will become increasingly important as CKAN itself continues to evolve.

There's also the question of how CKAN adapts to the changing expectations around data platforms. The world has moved a lot since 2006. Users expect richer interfaces, smarter data processing, and tighter integrations with the tools they already use. Some of that is already happening—extensions like `ckanext-charts`, `ckanext-admin-panel`, and `datapusher-plus` show that the community is actively pushing in these directions. But there's still a gap between what a modern data portal could be and what CKAN offers out of the box, and that gap is where extensions will continue to play their most important role.

I also think we'll see more consolidation. The pattern has repeated itself enough times—DataPusher to XLoader to `datapusher-plus`, `reclineview` to `datatablesview` to tables—that it's safe to say overlapping solutions will keep appearing and eventually converging. That's healthy. It means the ecosystem is alive and self-correcting.

Back in 2019, the Open Knowledge Foundation supported the joint stewardship proposal put forward by Link Digital and Datopian. Being appointed a co-steward of the CKAN project allows us to be a key part of ensuring the CKAN community thrives as an engaged and active group of believers. And we're going to continue supporting the CKAN ecosystem, to ensure that CKAN continues to thrive.

Twenty years is a long time for any open-source project. The fact that CKAN's ecosystem is still expanding, still experimenting, and still attracting contributors is not something to take for granted. Here's to the next chapter.
