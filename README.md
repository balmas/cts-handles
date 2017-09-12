The Canonical Text Services (CTS) Protocol defines a URN-based identifier structure for identifying texts and 
canonically cited passages of texts. It also defines a companion CTS Application Programming Interface (API) 
protocol for a service to retrieve fragments of texts by canonical reference, as expressed by their CTS URNs. 
The CTS URN syntax enables the expression of texts and passages of texts as first class, stably identified data objects. 
The CTS API allows the resolving of identifiers into the data that they represent.

The CTS URN specification was intentionally designed to be technology independent, but in a world where the web 
is currently the protocol of choice for resolution of data identifiers, lack of a standard approach to automatically 
linking CTS URNs with an instance or instances of the CTS APIs which can actually resolve them is a critical gap. 
The Handle System offers an attractive solution to this problem. According to the specification “The Handle System 
includes an open protocol, a namespace, and a reference implementation of the protocol. The protocol enables a
distributed computer system to store names, or handles, of digital resources and resolve those handles into the 
information necessary to locate, access, and otherwise make use of the resources.” Combining URNs with Handles 
should allow us to connect the precise CTS URN referencing capability with the flexible resolution capabilities 
and existing infrastructure of the widely-used Handle System.

We have [proposed a solution](proposal.md) using the Handle System as a centralized authority, registering a prefix 
for CTS URNs and leveraging the templating mechanism of the Handle system to map CTS URN namespaces to the addresses of their CTS API endpoint(s), thereby enabling URNs to be resolved to the specific texts and text passages they identify by client 
services with no specific knowledge of either the CTS API protocol or the individual publishers of each text. 
The Handles themselves will resolve to the CTS API endpoints, ensuring that their use returns the actual textual 
data thus identified. 



