
# Authors

Bridget Almas, Tufts University

Larry Lannom, CNRI

Robert Tupolo-Schneck, CNRI

[RPID Project](https://rpidproject.github.io/rpid/)

# Overview

The Canonical Text Services (CTS) Protocol defines a URN-based identifier structure for identifying texts and canonically cited passages of texts. It also defines a companion CTS Application Programming Interface (API) protocol for a service to retrieve fragments of texts by canonical reference, as expressed by their CTS URNs. The CTS URN syntax enables the expression of texts and passages of texts as first class, stably identified data objects. The CTS API allows the resolving of identifiers into the data that they represent.

The CTS URN specification was intentionally designed to be technology independent, but in a world where the web is currently the protocol of choice for resolution of data identifiers, lack of a standard approach to automatically linking CTS URNs with an instance or instances of the CTS APIs which can actually resolve them is a critical gap. 

Individual publishers of CTS URNs have taken different approaches to this problem, from a simple use of a domain name and rewrite rules on the supporting http server to redirect requests to an API endpoint (<sup>http://sites.tufts.edu/perseusupdates/beta-features/perseus-stable-uris/</sup>) to custom built resolution services (<sup>/http://doi.org/10.5334/dsj-2016-013</sup>).  However all of these solutions require that the consumer of a CTS URN know which publisher makes the identified text available, and where and how that particular publisher resolves those URNs.  A standard approach which allows for distributed publishing but centralized management of the resolution is required.

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

## Roles

* Centralized Handle System Provider (CHSP) - one or more organizations assuming responsiblity for registering and administering Handle prefixes for CTS Namespaces 

* Participating CTS Text Publisher (PCTP) - a publisher of CTS URN identified texts who wants their text URNs to be globally resolved by the Handle System

* hdl.handle.net provider (HDL) - the provider of the global hdl.handle.net proxy service 

* Non-Participating CTS Text Publisher (NPCTP) - a publisher of CTS URN identified texts who does not want to participate in the centralized solution but wants to publish a Handle for their text

## Responsibilities

* Centralized Handle System Provider (CHSP) 
    * registers Handle Prefixes at the level of the CTS Namespace
    * creates template handles for every CTS work and CTS version within a prefix that is registered with it by PCTPs
        * template handles at the level of the edition are necessary to support multiple publishers providing editions of the same work)
        * Neither passages nor subreferences would rate separate template handles, although nothing would prevent this being done if the future if the system proves useful and sustainable.
        * See Appendix 3: Sample Mappings for details of what the template handle regexes might look like
    * keeps HDL informed of all Handle prefix -> CTS Namespace mappings under its authority
    * either gives the PCTPs direct edit permissions on the relevant template handle(s) or serves as general purpose handle admin and develops an out of band mechanism for the PCTPs to keep it informed of changes.
    * provides a landing page for cases where a single URN resolves to multiple possible providers or service endpoints
    
* The hdl.handle.net proxy provider
   * Responsible for mapping CTS URNs to Handle Proxy urls. 
       * The hdl.handle.net proxy resolves potential handles in order to probe for the lowest level of granularity.  It first looks to see if there is an individual handle for the specific CTS edition URN, and if so it uses that. Otherwise it maps it to the prefix registered for the namespace and lets the Handle Service responsible for that prefix take over.
       * Supporting edition-specific handles from non-participating CTS Text Publishers requires support for a conventional handle syntax. This should be extensible but ideally limited to a few possible constructs.

* Participating CTS Text Publisher( PCTP)
    * informs the CHSP responsible for their namespace prefix of the list of CTS URNs for which they are going to provide services and the details of calling those services 
        * this may be done through direct management of a template handles or through out of band mechanisms according to the preference of the Handle CHSP
    * informs the CHSP of changes to their URN mappings
        * changes would include things such as the location of the API endpoint providing access to the edition or redirection of an previously published text to a different edition.

* Non-Participating CTS Text Publisher (NPCTP)
    * Are free to registered their own Handles for specific CTS editions without participating in the central organizational management. In order to take advantage of the proxy system, these Handles must adhere to a standard syntax.

## Basic Use Cases

### Use Case 1: Full URN with Edition and Passage

1. Consumer requests urn:cts:greeklit:tlg0012.tlg002.perseus-grc2:2.1 from the hdl.handle.net proxy

2. HDL looks for an edition specific handle, and finding none, maps it to the default handle for the greekLit namespace, inserting a delimiter at the point of the passage identifier.

    https://hdl.handle.net/20.500.20.20.20/urn:cts:greekLit:tlg0012.tlg002.perseus-grc2|:2.1 (where the 20.500.20.20.20 prefix is specific to the URN namespace greeklit)

3. The CHSP resolves the request to a template handle at

    20.500.20.20.20/urn:cts:greekLit:tlg0012.tlg002.perseus-grc2

4. Template handle maps the request to the URL for the GetPassage request at the provider's CTS API endpoint:

    https://cts.perseids.org/api/cts?request-GetPassage&urn=urn:cts:greeklit:tlg0012.tlg002.perseus-grc2:2.1
    
5. CHSP returns the URL to the HDL

6. HDL issues a redirect response to the consumer

![Use Case 1 Sequence](https://github.com/rpidproject/cts-handles/blob/master/ctsuc1.png?raw=true)

### Use Case 2: Full URN with Edition but no Passage

1. Consumer requests urn:cts:greeklit:tlg0012.tlg002.perseus-grc2 from HDL

2. HDL looks for an edition specific handle, and finding none, maps it to the default handle for the greekLit namespace, inserting a delimiter at the point of the (missing) passage identifier.

https://hdl.handle.net/20.500.20.20.20/urn:cts:greekLit:tlg0012.tlg002.perseus-grc2|

3. The CHSP resolves the request to a template handle at

    20.500.20.20.20/urn:cts:greekLit:tlg0012.tlg002.perseus-grc2

4. Template handle contains the logic of ‘no final passage component then...’ and so result in the redirect to a GetValidReff request at the provider's CTS API endpoint:

    http://cts.perseids.org/api/cts?request=GetValidReff&urn=urn:cts:greekLit:tlg0012.tlg002.perseus-grc2

5. CHSP returns the URL to the HDL

6. HDL issues a redirect response to the consumer

![Use Case 2 Sequence](https://github.com/rpidproject/cts-handles/blob/master/ctsuc2.png?raw=true)

### Use Case 3: Work level URN with passage

1. Consumer requests urn:cts:greekLit:tlg0012.tlg002:1.1 from HDL

2. HDL looks for an edition specific handle, and finding none, maps it to the default handle for the greekLit namespace, inserting a delimiter at the point of the passage identifier.

https://hdl.handle.net/20.500.20.20.20/urn:cts:greekLit:tlg0012.tlg002|1.1

3. The CHSP resolves the request to a different template handle at

    20.500.20.20.20/urn:cts:greekLit:tlg0012.tlg002

4. Template handle identifies the url for the GetPassage request:

    http://cts.perseids.org/api/cts?request=GetPassage&urn=urn:cts:greekLit:tlg0012.tlg002:1.1

5. CHSP returns the URL to the HDL

6. HDL issues a redirect response to the consumer

![Use Case 3 Sequence](https://github.com/rpidproject/cts-handles/blob/master/ctsuc3.png?raw=true)

### Use Case 4: Work level URN without passage

1. Consumer requests urn:cts:greekLit:tlg0012.tlg002 from HDL

2. HDL looks for an edition specific handle, and finding none, maps it to the default handle for the greekLit namespace, inserting a delimiter at the point of the (missing) passage identifier.

https://hdl.handle.net/20.500.20.20.20/urn:cts:greekLit:tlg0012.tlg002|

3. The CHSP resolves the request to the template handle at

    20.500.20.20.20/urn:cts:greekLit:tlg0012.tlg002

4. Template handle contains  the logic of ‘no passage...’ is applied and the resulting redirect is to a GetCapabilities request at the provider's endpoint:

    http://cts.perseids.org/api/cts?request=GetCapabilities&urn=urn:cts:greekLit:tlg0012.tlg002

5. CHSP returns the URL to the HDL

6. HDL issues a redirect response to the consumer

![Use Case 4 Sequence](https://github.com/rpidproject/cts-handles/blob/master/ctsuc4.png?raw=true)

## Advanced Use Cases

### Use Case 5: Multiple providers of the same URN

In the event multiple PCTP offer access to the same work or edition of a text, the CHSP can return a URL to a landing page from which the consumer can choose which link to follow.  The level of information desired for such a landing page will dictate the complexity of the implementation. In the simple end of the spectrum, the CHSP could keep a small amount of metadata about each PCTP and display that along with the links. A more complex solution might involve the CHSP retrieving metadata dynamically from the PCTP endpoints.

### Use Case 6: Passage specific template handles

A PCTP may publish incomplete editions of a work, in which only a subset of passages are available, or to offer a less granular citation hierarchy for a work (e.g offering passage retrieval at the level of a verse but not a line). 

The above solution, which assumes the most granualar template handle to be for a specific CTS version, would not take into account the passages a given endpoint has available for a requested work or edition before returning a URL, so it's possible the consumer would get redirected to an endpoint which does not have the requested passage. 

It is technically possible to register template handles which are more granular than a CTS version, but the overhead of keeping these handles up to date may or may not be worth the benefit. The most likely way of implementing such a solution might be to develop a service which issued a GetValidReff request to all registered CTS endpoints for all versions of a work, and then created and updated the template handles automatically with regexes which matched the passages offered by the endpoint.  

### Use Case 7: Data Type specific request/response

Some PCTP may use CTS URNs to identify their text, but not provide a CTS API Endpoint for those texts.  Or providers may offer extensions to the CTS API which support retrieval of the text in a variety of formats (XML, plain text, JSON, etc.). There would be various ways to enable consumers to explicitly request their desired response type.  Most solutions would impose requirements on all participants in the solution. One way to implement this would be to use the standard HTTP Accept header. The HDL proxy would need to pass this header on to the CHSP and then the template handles would need to be configured to take the appropriate path depending upon the match between a header in the request and the details of the available endpoint(s).

For example, the HTTP Accept header could be used to request a specific text mimetype:

```
curl -H Accept: text/xml cts.handle.net/urn:cts:greekLit:tlg0012.tlg0012.perseus-grc2:1.1
curl -H Accept: text/plain cts.handle.net/urn:cts:greekLit:tlg0012.tlg0012.perseus-grc2:1.1
```

In this case the PCTP would have to provide different urls per content mimetype and the CHSP would have to configure the template handles to make of the HTTP Header in its logic.

Custom HTTP header values could be used to enable a consumer to specify whether they only wanted to be directed to CTS API endpoints.  For example:

```
curl -H Accept: application/vnd.cite-architecture.cts+xml cts.handle.net/urn:cts:greekLit:tlg0012.tlg0012.perseus-grc2:1.1
```

In this case the CHSP might configure the template handle logic to examine the urls of the PCTP endpoints and exclude any that didn't adhere to the CTS API protocol.

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

## Scenario 2: CTS Functionality with Namespace-specific Handle Governance
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

# Appendix 3 - Sample Mappings 

[sample_mappings.md](sample_mappings.md)

# Appendix 4 - Sample CTS Requests/Responses

[sample_cts_request_response.md](sample_cts_request_response.md)

# Appendix 5 - Proposed Perseus Implementation

It should be possible to integrate the management of Handle records for the [Perseus Digital Library's](http://www.perseus.tufts.edu) CTS URNs into its [CapiTainS-based](http://capitains.org/) text publishing process. Perseus would act in the role of a Participating CTS Text Publisher (PCTP). 

Assuming that the CHSP provides an automated means to for PCTPs to create and update handles (either via the Handle Service HTTP API or a customized solution), Perseus could add steps to its text deployment and urn assignment processes to issue Create requests for new URN Handles and Update requests for previously registered URN which have been replaced by newer versions. 

Perseus texts are currently automatically deployed on the [Perseids instance of the CapiTainS CTS API](http://cts.perseids.org/api/cts/?request=GetCapabilities).  The deployment process is managed by [a cron-invoked script](https://github.com/Capitains/puppet-capitains/blob/master/templates/update_capitains_repos.rb.erb) which pulls release bundles (built by the [HookTest Continuous Integration system](https://github.com/capitains/hook)) from GitHub and publishes them to the CTS API endpoint.  This script could be modified to automatically update the Centralized Handle Service Provider (CHSP) to ensure that there is a single handle per work and edition/translation.

In addition to registering new handles from the CapiTainS release bundles, the process would also need to provide the ability to redirect previously published URNs which have been replaced by newer versions.  The process by which new URNs are assigned at Perseus uses the [Cite Collections API](https://github.com/PerseusDL/cite_collections_rails) of the [Perseus Catalog](http://catalog.perseus.org). (The API is deployed at http://catalog.perseus.org/cite-collections/api/.) If a new URN is created with the intention for it to replace an earlier published URN, this is identified through a `source_urn` property in the Cite Collection record for the URN.  

The overall process to update the Handle records might look something like this:

![Perseus CTS Handle Create/Update Process](https://github.com/rpidproject/cts-handles/blob/master/perseusctshandles.png)

In this scenario the Perseus Handle Script would be invoked from the automated process which deployes new CTS-identified texts from GitHub releases.

__Step 1__: Get the list of work and version level URNs included in the release

__Step 2__: Get the list of all handles for each CTS namespace in that list  (by specifying the prefix defined for that namespace, which would be communicated out of band between the CHSP and Perseus)

_Step 3 is repeated for each URN in the release:_

__Step 3__: Check to see if a handle exists for the URN. If so, proceed stop here and iterate to next URN.

 _Steps 4 - 7 are repeated for each URN in the release for which there is NOT already an existing Handle:_
 
__Step 4__: Retrieve the CITE record for the URN from the Perseus Catalog Cite Collection API

__Step 5__: Check to see if the URN is a replacement for an earlier version and that version also has a Handle. If so, proceed to Step 6. If not skip to Step 7.

__Step 6__: Update the HS_NAMESPACE value for Handle for the source URN (i.e. the URN which is being replaced) to redirect to the newer URN

__Step 7__: Create a new Handle for the new URN with the appropriate template in the HS_NAMESPACE record.



