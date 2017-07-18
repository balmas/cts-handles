
# Overview

The Canonical Text Services (CTS) Protocol defines a URN-based identifier structure for identifying texts and canonically cited passages of texts. It also defines a companion CTS Application Programming Interface (API) protocol for a service to retrieve fragments of texts by canonical reference, as expressed by their CTS URNs. The CTS URN syntax enables the expression of texts and passages of texts as first class, stably identified data objects. The CTS API allows the resolving of identifiers into the data that they represent.

The CTS URN specification was intentionally designed to be technology independent, but in a world where the web is currently the protocol of choice for resolution of data identifiers, lack of a standard approach to automatically linking CTS URNs with an instance or instances of the CTS APIs which can actually resolve them is a critical gap. 

Individual publishers of CTS URNs have taken different approaches to this problem, from a simple use of a domain name and rewrite rules on the supporting http server to redirect requests to an API endpoint (<sup>http://sites.tufts.edu/perseusupdates/beta-features/perseus-stable-uris/</sup>) to custom built resolution services (<sup>/http://doi.org/10.5334/dsj-2016-013</sup>).  However all of these solutions require that you know which publisher makes a CTS URN identified text available, and where and how that particular publisher resolves those URNs.  A standard approach which allows for distributed publishing but centralized management of the resolution is required.

Any solution to this problem must address both technical and the governance challenges. An established and reliable infrastructure is needed, one that not only provides some viable technical solutions but also enables a variety of 
governance structures to exist.

The Handle System is one solution that provides answers on both fronts.  The following characteristics of the Handle system (<sup>https://www.ietf.org/rfc/rfc3650.txt</sup>) are of particular importance:

      -  Persistence: Handles may be used as persistent identifiers for
         Internet resources. The only operational connection between a handle 
         and the entity it names is maintained within the Handle System.  
         This allows the same name to persist over changes of location, ownership, 
         and other state conditions.
         
      -  Multiple Instances: A single handle can refer to multiple
         instances of a resource, at different and possibly changing
         locations in a network.

      -  Multiple Attributes: A single handle can refer to multiple
         attributes of a resource, including associated services,
         available through any method at different and possibly changing
         network locations.  Handles can thus be used as persistent
         entry points into an evolving world of services associated with
         identified resources.

      -  International Support: The handle namespace is based on Unicode
         3.0 [17], which includes most of the characters currently used
         around the world.  
         
      -  Distributed Service Model: The Handle System defines a
         hierarchical service model such that any local handle namespace
         may be serviced by a corresponding local handle service, by the
         global service, or by both.  
         
      -  Distributed Administration Service: Each handle may define its
         own administrator(s) or administrator group(s).  Ownership of
         each handle is defined in terms of its administrator or
         administrator groups.  
         
      -  Efficient Resolution Service: The handle protocol is designed
         to allow highly efficient name resolution performance. 


Combining URNs with Handles should allow us to connect the precise CTS URN referencing capability with the flexible resolution capabilities and existing infrastructure of the widely-used Handle System.

The following proposes an approach to using the using the Handle System as a centralized authority for CTS URN resolution. It takes advantage of the Handle System's Template Handle feature (<sup>http://www.handle.net/tech_manual/HN_Tech_Manual_8.pd</sup>) to traverse the CTS URN structure and offer the service which matches the specificity of the URN requested. It also uses the Handle Systems global web proxy at hdl.handle.net to map URNs to their corresponding Handles, thereby enabling URNs to be resolved to the specific texts and text passages they identify by client services with no specific knowledge of either the CTS API protocol or the individual publishers of each text. The Handles themselves will resolve to the CTS API endpoints, ensuring that their use returns the actual textual data thus identified.

# Recommended Approach

* One or more central organizations are responsible for registriging a Handle Prefixes at the level of the CTS Namespace. 

* The hdl.handle.net proxy is responsible for mapping CTS URNs to Handle Proxy urls. The hdl.handle.net proxy resolves potential handles in order to probe for the lowest level of granularity  It first looks to see if there is an individual handle for the specific CTS edition URN, and if so it uses that. Otherwise it maps it to the prefix registered for the namespace and lets the Handle Service responsible for that prefix take over.

* Individual publishers are free to registered their own Handles for specific CTS editions without participating in the central organizational management. In order to take advantage of the proxy system, these Handles must adhere to a standard syntax.

* Neither passages nor subreferences would rate separate template handles, although nothing would prevent this being done if the future if the system proves useful and sustainable.

* CTS service providers wishing to have their texts be available via the managed Handle service tell the central organization responsible for their namespace prefix which works and which work/editions for which they are going to
provide services and the details of calling those services. 

* The central organization creates template handles for every work and edition within a prefix that is registered with it by the service providers. 
    * template handles at the level of the edition are necessary to support multiple publishers providing editions of the same work
    * QUESTION for Robert/Larry: does the handle service provide a way for user selection if multiple handles match? i.e. if a request for a work comes in and multiple providers offer versions of that work?

* The central organization can either give the relevant publishing organization edit permissions on the relevant template handle(s) (assuming they want it, which means they want to understand how it works and maintain it over time). or serve as general purpose handle admin and the CTS service providers would have to keep them informed of changes.
    * changes would include things such as the location of the API endpoint providing access to the edition or redirection of an previously published text to a different edition.


Fulfillment of Use Cases:

Use Case 1: Full URN with Edition and Passage

urn:cts:greeklit:tlg0012.tlg002.perseus-grc2:2.1

would get mapped to something like

https://hdl.handle.net/20.500.20.20.20/urn:cts:greekLit:tlg0012.tlg002.perseus-grc2:2.1 (where the 20.500.20.20.20 prefix is specific to the URN namespace greeklit)

which would resolve to a template handle at

20.500.20.20.20/urn:cts:greekLit:tlg0012.tlg002.perseus-grc2

which would result in a redirect to a GetPassage request at the provider's CTS API endpoint:

https://cts.perseids.org/api/cts?request-GetPassage&urn=urn:cts:greeklit:tlg0012.tlg002.perseus-grc2:2.1

and would work for any possible sequence of passages. 

Use Case 2: Full URN with Edition but no Passage

urn:cts:greeklit:tlg0012.tlg002.perseus-grc2

which is the edition only without the passage and would get mapped to

https://hdl.handle.net/20.500.20.20.20/urn:cts:greekLit:tlg0012.tlg002.perseus-grc2

which would resolve to the same template handle as in Use Case 1, which would contain the logic of ‘no final passage component then...’ and so result in the redirect to a GetValidReff request at the provider's CTS API endpoint:

http://cts.perseids.org/api/cts?request=GetValidReff&urn=urn:cts:greekLit:tlg0012.tlg002.perseus-grc2

Use Case 3: Work level URN with passage

urn:cts:greekLit:tlg0012.tlg002:1.1

This maps to a different template handle, first being mapped to

https://hdl.handle.net/20.500.20.20.20/urn:cts:greekLit:tlg0012.tlg002:1.1

which resolves to the template handle

20.500.20.20.20/tlg0012.tlg002

which redirects to a GetPassage request:

http://cts.perseids.org/api/cts?request=GetPassage&urn=urn:cts:greekLit:tlg0012.tlg002:1.1

Use Case 4: Work level URN without passage

urn:cts:greekLit:tlg0012.tlg002

maps to

https://hdl.handle.net/20.500.20.20.20/urn:cts:greekLit:tlg0012.tlg002

which resolves to the same template handle as in Use Case 3

20.500.20.20.20/tlg0012.tlg002 where the logic of ‘no passage...’ is applied and the resulting redirect is to a GetCapabilities request at the provider's endpoint:

http://cts.perseids.org/api/cts?request=GetCapabilities&urn=urn:cts:greekLit:tlg0012.tlg002


# Appendix 1 - User Stories

US 1 As a user I want a CTS Work URN (with a passage reference) to resolve to a list of URLs serving editions and translations of this work. 

US 2 As a user I want a CTS Work URN (without a passage reference) to resolve to a list of URLs serving editions and translations of this work. 

US 3 As a user I want a CTS Edition/Translation URN without a passage reference to resolve to a list of URLs at which I can retrieve passages from this edition/translation. user story

US 4 As a user I want a CTS Edition/Translation URN with a passage reference to resolve to a list of URLs at which I can retrieve that passage. 

US 5 As a user I want a CTS Edition/Translation URN which has been retired/replaced to resolve to a list of URLs for the replacement edition/translation.

US 6 As a publisher I want to assert responsibility for publishing textgroup and work urns within a given namespace.

US 7 As a publisher I want to be able to publish a edition or translation URN for a text that in a namespace that is managed by another publisher. 

US 8 As a publisher I want to be able to retire an edition or translation URN that I previously published and identify a URN for its replacement edition or translation. 

US 9 As a publisher I want to be able to retire an edition or translation URN that I previously published without providing a replacement edition or translation. 

US 10 As a publisher I want to be able to provide a single URL at which all CTS URN identified texts I publish are available. 

US 11 As a publisher I want to be able to provide URLs at which subsets of CTS URN identified texts I publish are available.

US 12 As a publisher I want to specify whether the URL a CTS URN I publish is a CTS API Endpoint, and if so, which API version. 

# Appendix 2 - Scenarios Considered

## Scenario 1: Minimal Functionality, Minimal Handle Governance (CTS URNs direct to Handles)

In this scenario a Proxy Server is responsible for mapping CTS URNs to Handles and returning the Handles directly to the client.  Clients submit the URN to the proxy service and get back one or more Handles. Minimal to no support would provided for determining how to offer clients of the Proxy a choice between multiple Handle options.  Publishers of texts identified by CTS URN can run their own Handle Server to assign Handles or they can request Handles from a 3rd party provider.  They submit URNRegex-to-Handle mappings to the provider of the Proxy Server in order to enable their URNs to be resolved. It’s up to the publisher to decide what URL(s) to put in the Handle record for a CTS Text - these could be direct URLs to the text at the CTS API endpoint, or to a Landing Page,  or a download of the xml file.

The owner of the Proxy Server is committing to make the service available. CTS Publishers who submit Handle mappings to the Proxy Server provider are committing to keep their texts available at the Handle locations. CTS Text Publishers, if running a Handle Server, are committing to keep it available for any Handles they issue.  The Proxy Service could be provided by something as simple as w3id.org in this scenario.
  
We could establish some Best Practices for registering Handles for CTS-Identified texts. (E.g. If a Text Provider wants to return a CTS URN, and replace it with a newer version, they should get a new Handle for the newer text and update the mappings they send to the CTS Proxy Provider to have them redirect to the newer Handle.) 

## CTS Functionality with Namespace-specific Handle Governance
In this scenario the Proxy Server maps CTS URNs to CTS API Endpoints that offer the requested text. The Proxy Service can return any of the base CTS Endpoint URL or the specific API URLs to retrieve the text from the endpoint.  The reason to support both would allow for existing CTS API Client code to work as well as for non CTS-aware clients to just get whatever is provided from the API endpoint.  CTS-aware clients of the Proxy Service can use the CTS API Endpoints directly to make decisions about text to retrieve when multiple are available.

The Proxy Service Mapping is done based upon CTS Namespace to Handle Prefix, delegating responsibility for mappings within a Namespace to the Handle Service provider.  CTS Namespace Owners run Handle Servers which manage the URNs under their namespace(s), registering a Handle Prefix per Namespace and using Handle Templates to map URNs automatically to the CTS API Endpoints that provide the texts.  Individual publishers of texts under a managed namespace can submit their endpoints to the Namespace provider and request Handles for their texts.  Namespace owners who want their texts available via the CTS Handle Proxy submit their prefixes to the Proxy Service Provider. 

The owner of the Proxy Server is committing to make the service available. CTS Namespace providers who participate in the system are committing to running Handle Servers for their namespaces, sustaining the payment for their prefixes, and enabling individual CTS Text Publishers to request Handles for any texts they publish under that namespace. Governance policies need to be established between Namespace providers and Text Publishers.     

Sample Handle templates for CTS Namespace providers could be developed which map at various levels of the URN, with the simplest being to return a single CTS API endpoint for all texts. A really sophisticated implementation would update the Handle template automatically based upon the texts and passages that are available at an endpoint.

## Scenario 3: Advanced CTS+ Functionality with Namespace-specific Handle Governance
This is the same as scenario 2, except that in addition to supporting CTS API Endpoint URLs in the Handle Record, it would be possible to return a regular URL to the text, with support for various media types, using either Content Negotiation headers or format extensions. The Proxy Server would return the right format based upon what the client requested.  This could be used both by Text Publishers who provided a CTS API Endpoint for their texts and those that didn’t.

It would enable request to the proxy server such as:

curl -H Accept: text/xml cts.handle.net/urn:cts:greekLit:tlg0012.tlg0012.perseus-grc2:1.1
curl -H Accept: text/plain cts.handle.net/urn:cts:greekLit:tlg0012.tlg0012.perseus-grc2:1.1
curl -H X-CTS-Request cts.handle.net/urn:cts:greekLit:tlg0012.tlg0012.perseus-grc2:1.1
curl -H X-CTS-Endpoints cts.handle.net/urn:cts:greekLit:tlg0012.tlg0012.perseus-grc2:1.1

The Proxy Server could be responsible for translating CTS API URLs in the Handle Records to the requested format and CTS Text Publishers who didn’t supply a CTS Endpoint would just supply URLs to their content.
