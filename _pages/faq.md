---
permalink: /faq/
title: "FAQ"
overview: false
layout: single

sidebar:
  nav: "faq_nav"

toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
toc_sticky : true
---

## Frequently Asked Questions for One Data Model (OneDM)

### Who is behind OneDM? Where does OneDM come from?

In the late fall of 2018, the ZigBee Alliance hosted a gathering of IoT industry leaders (Hive), including other IoT standards organizations, to get together and have a frank and open discussion about the Internet of Things; what is the current status, what are the big challenges ahead?

By far, the most frequently cited challenge was the inconsistency and lack of interoperability across the field of IoT data models. The lack of a common data model for IoT devices was seen as the biggest challenge; competition between data models is the biggest impediment to growth of the industry. The industry leaders at Hive 2018 decided to collectively work through the differences in IoT data models and deliver a converged, harmonized IoT data model to industry.

One Data Model (OneDM) was started in early 2019, consisting of several IoT SDOs (Standards Development Organizations) and IoT device and platform vendors, operating under a broad, multi-party liaison agreement.

### What is the problem being solved by OneDM, and how is it being solved?

OneDM is solving the problem of lack of a common data model for IoT and IoT devices. Each IoT standards organization, and many IoT platform vendors, have created their own version of an IoT data model framework, each with a bespoke meta-model and representation language. Some have used XML serialization, some JSON, and some have created formats based on other data languages and tools, such as OpenAPI (Swagger).

Despite the divergence of representation languages, the underlying meta-information-models are strikingly similar, conceptually and semantically. They mostly vary syntactically and use diverse vocabulary constructs to represent similar concepts. The devices and functionality being modeled are also very similar and are described at a similar level of abstraction across the field.

The OneDM group decided to undertake a comprehensive review of the state of the art in IoT data models, to see if a common data model could be based on a down-selection from the existing IoT data models. While many of the models that were evaluated have strong suits, it was concluded that no single data model was sufficient to form the basis of a common IoT data model across industry. The more mature models have insufficient coverage across application domains, and the tools and formats being used are not well enough developed in any one area to meet the broad set of requirements.

Based on the industry review, the OneDM group decided to split the work into two phases, first the development of a common modeling framework and language that can describe and represent all of the existing IoT data models with an extensible architecture, offering good support for modern tools, and being friendly for developers. In this phase, we have subjected the proposed language and framework to a pressure test, representing models from diverse sources to identify common optimizations and common design patterns that need to be supported.

In the first phase, we enable diverse IoT data models to be contributed in a common format, with the features and limitations they come with from their parent organizations. All of the contribution organizations have agreed to make their contributions under the BSD 3-Clause open source license to enable OneDM converged models to be derived works of the original contributions.

The first phase result is the Semantic Definition Format (SDF) and related tools and frameworks to manage the contribution of data models from diverse sources, and collection of these data models in a single place.

The second phase is the normalization of IoT data models to select a common set of device and functionality definitions that can be used to create interoperable devices and services across IoT vendors and standards organizations. In this phase, we develop policies and processes for building consensus around a common set of data model definitions that everyone can use to model and define a common set of IoT devices and services. The result of phase two will be a uniform set of high level, protocol agnostic, IoT data models that can be used to generate semantically interoperable devices and services that can be deployed on a single network protocol or bridged across protocols.

### What is the relationship of OneDM to other IoT standards organizations?  How is OneDM not just another competing IoT standard?

The purpose of OneDM is not to compete with any existing standards body, but rather to provide a common set of tools and models that can be used by all bodies and organizations. Rather than attempting to provide a "better" alternative, OneDM sets out to bring forward the best of several contributing organizations, and then provide the resulting converged models and underlying tools back for everyone to use.

The tools and formats developed by OneDM are a collaborative work product of several IoT organizations and embody the best thinking in IoT data modeling. This is enabled by the contributors' agreement to make it easier for our collective markets to develop and grow based on a common interoperable technology instead of trying to compete on data models. We decided to collaborate where there is little differentiation to be exploited, and compete where we can find more value in developing new system features.

OneDM will be open for anyone to join and contribute, and all of the tools and models will be available under open source license terms.

### How do I as a developer use OneDM?

OneDM provides a modeling language, [Semantic Definition Format (SDF)][SDF language], and provides access to a body of contributed IoT data models that you can use to build IoT devices. OneDM provides some basic tools to enable you as a developer to define new models, translate models to and from other SDO formats like LwM2M XML/MOD files or OCF OpenAPI files. The models and tools are all available under the BSD 3-Clause license.

As an organization or vendor, you can use OneDM to host a privately-managed repository where you can manage your own online stable namespace within OneDM for your own definitions, whether you ultimately intend to converge them in OneDM or not.

### How do I contribute new definitions to OneDM?

IoT definitions can be uploaded to OneDM by making pull requests of SDF files to a OneDM repository. Initially, OneDM hosts two repositories, the [playground][] for work on convergence of common definitions, and an [exploratory][] repository to work on new language features, extension points, and design patterns. Additionally, new OneDM repositories will be created for definitions to be managed by organizations outside OneDM.

As OneDM moves into the model convergence phase, you can contribute proposed definitions and features and participate in the model down-selection process that will be conducted in open meetings and teleconferences.

### What license is oneDM using for its data models?

All models contributed should have BSD-3 licence.
In this way the in and out licence of the acceptance process will have the same licence.

### How do I get involved in the development of OneDM?

OneDM, as an organization, is governed by a set of participants that operate under a broad liaison agreement. As an organization, you can join OneDM and participate in the liaison activities subject to the liaison agreement. This includes deciding on governance policies.

As an independent organization or developer, you can participate in the open OneDM meetings and teleconferences, submit Github issues and make pull requests, and in general contribute freely to the development of OneDM as an open source community project subject to OneDM project governance policies.

## Frequently Asked Questions for the Semantic Definition Format (SDF)

### There are so many IoT languages and data formats. Why did OneDM create a new format (SDF) rather than reuse an existing format for IoT data models?

The OneDM prior art review considered at least 6 different languages and formats for SDF. None of the existing formats covered the requirements well enough to warrant direct adoption. The requirements include:
- Semantic Integration. Provide stable URIs to map concepts
- Namespace support. Enable organizations to contribute and manage models in disparate namespaces to avoid conflicts and enable decoupled work streams. Models can be merged by mixing namespace references. Definitions can be adopted into new namespaces by changing prefixes.
- Developer-friendly language and format that enables common tools and patterns to be used. Strongly favor JSON for tools and developer support
- Extension points to enable manufacturer-specific definitions and language evolution while preserving the ability for strong validation
- Architecture driven by schema or shape constraints
- Strong composition ability, model mash-ups
- Ability to model high level composed organizations of devices and services

### What makes SDF different from the recent W3C Web of Things recommendation for WoT TD (Thing Description)?

- Both frameworks define thing "affordances" characterized as Properties, Actions, and Events
- SDF is meant to define application types by mapping vocabulary terms to definitions. For example, SDF can describe common affordance types for all light switches (on/off state, toggle actions).
- SDF is protocol-agnostic and has no defined protocol binding
- SDF is payload-format agnostic and only defines constraints on individual data items (no data schemas for payloads)
- WoT TD is meant to describe instances of things and contains payload schemas and protocol bindings for affordances that might be named in SDF definitions.
- WoT TD is not meant to provide the application type vocabularies; SDF can define the vocabularies and provide URI references as semantic anchors for terms that are used in WoT TDs.
- SDF can provide the semantic anchors for abstract application types (e.g., on/off switch or motion sensor) and TD can provide the instance definitions, payload schemas, and protocol bindings with network addresses for the affordances.

### Why is SDF represented in JSON and not JSON-LD for better semantic integration?

- SDF is represented in JSON to enable developers to use JSON tools for the basic semantic integration, providing anchor URIs for defined terms that represent application data model concepts, and providing JSON based key/value metadata for definition elements (SDF "qualities").
- The use of JSON enables familiar idiomatic patterns and tools to be used in construction and processing of SDF definitions. Schema driven editors, text editor plugins, schema validation, and schema-driven constructors are already available to assist in SDF oriented workflows.

### How does the SDF terminology relates to other organizational usages?

The relationship between SDF [terminology][] and other SDO terminology is explained.

### Is there a video tutorial?

A tutorial is available on [![youtube](https://img.youtube.com/vi/sTrqa5jYVKo/0.jpg)](https://www.youtube.com/watch?v=sTrqa5jYVKo)
Note that the tutorial uses the old prefix odm instead of sdf.

### Is there a presentation available?

<object data="/assets/pdfs/SDF-Semantics_2020-07-18.pdf" type="application/pdf" width="700px" height="700px">
    <embed src="/assets/pdfs/SDF-Semantics_2020-07-18.pdf">
        <p>This browser does not support PDFs. Please download the PDF to view it: <a href="/assets/pdfs/SDF-Semantics_2020-07-18.pdf">Download PDF</a>.</p>
    </embed>
</object>

<!--  LocalWords:  affordances namespace schemas SDF SDOs ZigBee SDO
 -->

[SDF]: https://github.com/one-data-model/SDF
[tools]: https://github.com/one-data-model/tools
[playground]: https://github.com/one-data-model/playground
[exploratory]: https://github.com/one-data-model/exploratory
[unit_test]: https://github.com/one-data-model/unit_test


[SDF language]: https://onedm.org/SDF/sdf.html

[terminology]: /terminology

[RFC8610]: https://tools.ietf.org/html/rfc8610

<!--  LocalWords:  affordances namespace schemas SDF SDOs ZigBee SDO
 -->
<!--  LocalWords:  OpenAPI LwM OCF github namespaces WoT TDs LD
 -->



