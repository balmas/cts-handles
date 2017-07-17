
 TODO  to write an explanation of why we were dragging the handle infrastructure into this mess 
 in the first place - why not just write a general service that did the same thing. The general answer 
 is that an existing reliable infrastructure is needed, one that not only provides some viable technical 
 solutions but also  enables a variety of governance structures to exist, as governance is likely to be 
 the toughest problem to solve.

# Recommended Governance Scenario

Assumptions:

* A single organizational unit (e.g. maybe a shared infrastructure provider such as CLARIN) is in charge of all 
of the template handles and relevant prefixes to make this work. 

* The service/application that maps the CTS URNs to handle proxy URLs can also resolve potential  handles in order 
to probe for the lowest level of granularity. If there is a template handle for a specific edition, 
use that. Otherwise work your way up the hierarchy built into the work component.

* Neither passages nor subreferences would rate separate template handles, although nothing would prevent this.

Implementation Details:

Every CTS namespace, e.g., greeklit, has a corresponding handle prefix and that prefix points to the handle service
run by the central organization. The central organization also creates template handles for every work and work/edition 
within a prefix, that is needed for the total collection of participating CTS URN service providers. 

The CTS URN service providers tell the central organization which works and which work/editions for which they are going to
provide services and the details of calling those services. 

The central organization sets up the template handles appropriately and gives the relevant organization edit permissions 
on the relevant template handle(s), assuming they want it, which means they want to understand how it works and maintain it 
over time. Otherwise the central organization could serve as general purpose handle admin and the CTS service providers would have 
to keep them informed. 

Publishing requirements:

 [ To do - from slack For example to support scenario 3 , the handle template would need regexes which matched specific editions of a text and sent the user to a different repository depending upon which edition they requested.  Anytime a new edition was published,  whether to replace an existing edition, or to provide a totally new one, e.g. by a new publisher, the maintainer of the handle template would need to be informed and the regex would need to be updated.]

Fulfillment of Use Cases:

Use Case 1: Full URN with Edition and Passage

urn:cts:greeklit:tlg0012.tlg002.perseus-grc2:2.1

would get mapped to something like

https://hdl.handle.net/20.500.20.20.20/tlg0012.tlg002.perseus-grc2/2.1 (where the 20.500.20.20.20 prefix is specific to the
URN namespace greeklit)

which would resolve to a template handle at

20.500.20.20.20/tlg0012.tlg002.perseus-grc2

which would result in a redirect to

https://cts.perseids.org/api/cts?request-GetPassage&urn=urn:cts:greeklit:tlg0012.tlg002.perseus-grc2:2.1

and would work for any possible sequence of passages. 

The advantage is that editing the single template handle would accommodate any changes to the API and/or the 
location of the API service.

Use Case 2: Full URN with Edition but no Passage

urn:cts:greeklit:tlg0012.tlg002.perseus-grc2

which is the edition only without the passage and would get mapped to

https://hdl.handle.net/20.500.20.20.20/tlg0012.tlg002.perseus-grc2

which would resolve to the same template handle, which would contain the logic of ‘no final passage component then...’ 
and so result in the redirect of

http://cts.perseids.org/api/cts?request=GetValidReff&urn=urn:cts:greekLit:tlg0012.tlg002.perseus-grc2

Use Case 3: Work level URN with passage

urn:cts:greekLit:tlg0012.tlg002:1.1

This maps to a different template handle, first being mapped to

https://hdl.handle.net/20.500.20.20.20/tlg0012.tlg002/1.1

which resolves to the template handle

20.500.20.20.20/tlg0012.tlg002

which redirects to

http://cts.perseids.org/api/cts?request=GetPassage&urn=urn:cts:greekLit:tlg0012.tlg002:1.1

Use Case 4: Work level URN without passage

urn:cts:greekLit:tlg0012.tlg002

maps to

https://hdl.handle.net/20.500.20.20.20/tlg0012.tlg002

which resolves to the same template handle as in Use Case 3

20.500.20.20.20/tlg0012.tlg002 where the logic of ‘no passage...’ is applied and the resulting redirect is

http://cts.perseids.org/api/cts?request=GetCapabilities&urn=urn:cts:greekLit:tlg0012




At the other end of the continuum the CTS service providers could be given the tickets to mint and 
maintain all of the potential template handles, probably in a handle service they maintained.
