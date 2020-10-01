### Server hierarchy
* Root servers
* Top Level Domain (TLD) servers
    * .com, .edu, etc.
    * managed professionally
* Authoritative DNS servers
    * Actually store the name to address mappings.
    * Maintained by the corresponding authority.

Each server stores a subset of the total DNS namespace. An *authoritative* name server stores _resource records_ for all names in the domain it has authority over.

Each server can discover the server(s) that are responsible for the other portions of the hierarchy.

* Every server knows the root
* Root server knows the TLDs