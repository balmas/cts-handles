
 TODO  to write an explanation of why we were dragging the handle infrastructure into this mess 
 in the first place - why not just write a general service that did the same thing. The general answer 
 is that an existing reliable infrastructure is needed, one that not only provides some viable technical 
 solutions but also  enables a variety of governance structures to exist, as governance is likely to be 
 the toughest problem to solve.

# Recommended Approach

* A Handle Prefix is registered for every CTS Namespace. These namespace level handles are managed by one or more central organizations.

* Individual publishers are free to registered their own Handles for specific CTS editions without participating in the central organizational management.

* The hdl.handle.net proxy is responsible for mapping CTS URNs to Handle Proxy urls. The hdl.handle.net proxy resolves potential handles in order to probe for the lowest level of granularity  It first looks to see if there is an individual handle for the specific CTS edition URN, and if so it uses that. Otherwise it maps it to the prefix registered for the namespace and lets the Handle Service responsible for that prefix take over.

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


