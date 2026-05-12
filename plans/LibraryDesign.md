# Translator Component Toolkit (TCT) — a top-down design

**Key members:** Chunlei, Guangrong, Yue, Willow, Evan, Gaurav
**Working group:** Core Component Working Group

**Background:** In the Translator consortium, a set of tools have been developed to support the Translator overall infrastructure, including separate KGs, centralized tier 0 graph, TRAPI standard, Biolink tools, Name Resolver, Node Normalizer, Node Annotator, query functions, pathfinder functions, etc. All the tools can be re-usable for internal developers and power users for the Translator project. We have currently developed a Translator Component Toolkit (TCT), which is a single repo that provides access to all the components necessary for power users.

**Main problem to solve:** We have identified three groups of developers who need access to different kinds of components that are all currently found inside TCT, but who may not like to install all of TCT to get access to them:
* All Translator developers would benefit from having a fast, simple set of Python classes that can be used to store and validate essential Translator data structures like TRAPI nodes, edges, and annotations.
* Anybody interested in building tools on top of Translator would like programmatic access to the Translator Core Components (Name Resolver, Node Normalizer, Node Annotator) as well as libraries for simplifying common Translator tasks (constructing a TRAPI query and submitting it to a TRAPI endpoint, such as Retriever or the ARS).
* Power users would like to be able to make complex Translator queries using TCT (Neighborhood Finder, Pathfinder) without having to worry about the underlying steps necessary to resolve their search query to a CURIE, generate a TRAPI query, provide useful debugging information in the case of incorrect results, and provide formatting the response in a way that is usable for visualization and for integrating into scientific pipelines.

We therefore propose dividing TCT into three separately installable Python libraries: a lowest-level library that provides data objects for TRAPI (the TRAPI Object Model or TOM), a mid-level library that provides Python interfaces to core components and returns TOM objects as responses (Translator SDK), and a high-level toolkit that provides complex Translator functionality by calling the Translator SDK (Translator Component Toolkit). Currently, all functions of the Translator SDK are the same as the TCT (a simple copy of some functions in TCT). In the longer run, we will coordinate the TCT and Translator SDK development to better serve the consortium and power users. We also designed a better organization for TCT, see approach.

**Approach:** we designed three layers (similar to an onion) for these three layers:
1. Inner layer: TOM (TRAPI object model)
2. Intermediate layer: TSDK (Translator SDK) will provide 1:1 encapsulation of API endpoints from Translator services across all environments or custom URLs
   * Example functions:
     1. NodeNorm
     2. Node Annotator
     3. NameRes
     4. Tier 1/0 graph query (Retriever) – actually generating TRAPI queries will not be part of TSDK, but will be part of TCT
     5. ARS (but there are concerns about someone overusing/abusing/DDoSing the ARS)
3. Higher layer: TCT (neighborhood finder, Pathfinder, network annotator etc) will provide functionality needed by power users
   * Example functions:
     1. Query functions
     2. Neighborhood finder
     3. Pathfinder
     4. Network annotator
     5. Graph manipulation
     6. MCP related functions

**Integration and deployment**
1. TOM will set up an independent Python library (proposed key developer: Willow, Chunlei)
   * In development at https://github.com/NCATSTranslator/TRAPIObjectModeling, Willow expects to be able to return to this soon.
2. TranslatorSDK will be set up as an independent python library (It will start with the set of utility functions extracted from the current TCT repo. TCT will be updated to call these functions from the SDK. The future development will be coordinated with TCT to keep consistent.) (Proposed key developers: Yue, Gaurav, Evan, Willow, Chunlei, Guangrong + other contributors)
   * https://github.com/NCATSTranslator/Translator_sdk → to be renamed to TranslatorSDK
3. TCT will incorporate TOM (when it becomes mature), Translator_SDK and other utilities as a python library. (TCT will serve as a public library to engage with power users) (Proposed key developers: Guangrong, Yue, Gaurav, Chunlei, +other contributors)  
   (Note: the order of key developers are random)
   * https://github.com/NCATSTranslator/Translator_component_toolkit

**Branding discussion**
When engaging with users, we need to keep things simple and straightforward, so the TCT will be kept as a single Python package to guide users how to use Translator tools, with both TOM and SDK as its dependencies. When users install TCT, both TOM and SDK will be installed and imported via TCT (allowing TOM and SDK functions to be executed directly from within TCT).
