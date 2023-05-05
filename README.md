### Digital Commons Co-operative (DCC)
 
Welcome to the Github Organisation of the Digital Commons Co-operative(DCC). Our website can be found at https://digitalcommons.coop

### Quick summary of DCC's Tech stack.

There are some general infrastructure things, but the majority of it is for periodic collection of data to map from various external sources, including LimeSurvey questionnaires, Google Sheets, Nextcloud/Only Office sheets, CSV files, etc. and normalising it as CSV and semantic web linked data, which is then served back to the MykoMap instances, either as CSV files or via a SPARQL query server, and of course, hosting and monitoring the maps.

**GitHub** 
Here on Github itself we have a long list of projects. Notable:

- MykoMap - an embeddable map of points with attributes, searchable and filterable - Typescript/D3.
- open-data - a collection of data collection scripts constituting the "Sausage Machine" (see below) - Ruby and shell script
- map-sse - ESSGLOBAL SKOS vocabs, basically a taxonomy system for Co-ops and the Solidarity Economy, using semantic web data standards - RDF/TTL/shell/Ruby
- lod.coop - permanent URL redirection service, see below - Shell/HTML/PHP/Apache htaccess files
- Land Explorer - a mapping application for exploring and annotating land usage
  -  front end - React/Typescript
  -  back end - typescript/Postgres (was PHP/.Net/SQLServer)
- TWINE - app for managing teams of volunteers - Typescript/React/Postgres
- technical-infrastructure
   - ansible scripts for deploying dev-1/prod-1 (see below)
- password-store - GPG encrypted `pass` secrets for ansible, mostly
- goth - experimental RDF documentation library - Ruby
- shipshape - Mykomap config interrogation service - Javascript/Gatsby/PHP
- hookrunner - minimal build service web server - Javascript/PHP/Shell

**On WebArchitects**
 email/calendaring server, which handles DCC mail.

**A Hetzner account.**
We have a Hetzner server running Cloudron on Ubuntu, at https://my.digitalcommons.coop/
This provides us with:

- A website for digitalcommons.coop which runs on Ghost.
- A Matrix server, with an Element front end site.
- A Nextcloud server (although most docs are on Google)
- A OnlyOffice server for NextCloud
- An uptime Kuma service monitor
- An Umami website analytics server
- Backups for the above.
- Assorted other apps which we are evaluating or don't yet use very seriously.
- Hosting for LandExplorer

**A BitWarden account**
A people-friendly password store

**A Landex Cloudways server**
Hosting for the old Land Explorer land registry borders microservice

**Google Apps**
A Google Apps account which we use for sharing files and documents.

**Limesurvey**
 A LimeSurvey account on which the questionnaires gathering information for several of the earliest MykoMap sites. This is a source of initiatives data for the Sausage 

### SEA 
We are still using some services hosted by on of our founders Solidarity Economy Association.

**On WebArchitects**
 - wp-1 and wp-2 - wordpress installatons on the slidarityeconomy.coop domain
 - A zonefile service for the solidarityeconomy.coop domain

**Scaleway**
- Two Scaleway VMs running Ubuntu Linux, dev-1 and prod-1
- These are our build and deploy servers.
- They are essentially identical, but one is for production services, the other for development.
- These will be move to Hetzner at a convenient time.
- They run the following services (the same on each):
- Apache, which hosts the following websites on the solidarityeconomy.coop domain:
  - the servers primary domain
  - on dev-1 this provides access to
    - the "shipshape" app, which collects and displays the deployed configurations of the MykoMaps
    - the "hookrunner" app, which is a new and minimal build server triggered by webhooks, mainly for rebuilding MykoMap sites
  -  prod-1 does not currently have these apps
  -  the subdomains for currenllty hosted MykoMaps, with dev/prod prefix as appropriate (prod prefix optional):maps - more map sites which don't warrant their own domains
  - vocabs - hosts semantic web SKOS vocab definitions, mostly the ESSGLOBAL definitions, used by MykoMap linked data
  - data - hosts various sorts of static datasets for MykoMap, each including
    - RDF linked data in .ttl and .rdf files
    - CSV data
    - HTML renderings of the linked data with an index page
    - Some additional JSON data in some cases
  - A few others, either unused or aliases of the above
    - for
    - forsolidarity
    - dotcoops-coopsuk
    - survey
    - w3id
  - the lod.coop subdomain, with dev/prod prefix as appropriate (prod prefix optional) This houses a permanent-URL redirection service, akin to purl.org or w3id.org, but for DCC/co-op related purposes. Typically redirects https://lod.coop/foo/bar/... -> https://data.solidarityeconomy.coop/foo/bar/..., with some accepted content awareness.
   - Virtuoso, which has triplestore database and SPARQL endpoint for those MykoMaps which use them, accessed via HTTP on port 8890. There is a triplestore graph for most of the MykoMaps 
   - A cron job which runs the "Sausage Factory" AKA the "Data Factory". This is a Ruby-based data collection and processing service. E        external solidarity initiative data sources are polled for new data. New data is downloaded and converted into a standard CSV format. Addresses are geocoded to get location data. This is then converted into RDF/TTL using the ESSGLOBAL vocabulary and others the resulting static files is used to populate the `data` domains above. The RDF is imported into Virtuoso to populate the triplestores.




