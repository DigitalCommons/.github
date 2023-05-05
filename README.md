### Digital Commons Co-operative (DCC)
 
Welcome to the Github Organisation of the Digital Commons Co-operative(DCC). Our website can be found at https://digitalcommons.coop

### Quick summary of DCC's Tech stack.

This comes in these broad categories:
- General infrastructure things (organisational communications and other services, monitoring and development).
- The Land Explorer application, for exploring land usage and annotating it.
- MykoMap installations for showing directories of things (typically organisations or services).
- Systems for periodic collection of data from various external sources for these maps.

Land Explorer consumes data from the Land Registry and other open data sources. It can also store user-generated data. It also needs a map tile server, which it gets through the external MapBox service.

MykoMap can directly consume CSV from any accessible source on the web, or query one of the DCC maintained SPARQL query servers. It also needs a map tile server such as Open Street Map, which is generally hosted elsewhere.

The data collection services (AKA the "Sausage Machine") for MykoMaps normalise the data and convert it to CSV files, plus semantic-web linked data with a standard schema, which is imported into a triple-store database for SPARQL queries. The intended use for SPARQL is to make more complex queries and combine data sources, but also to allow third parties to consume the data.

A MykoMap dataset can be created any arbitrary source which can be converted into a CSV file with at least a stable identifier, a name, and (optionally) latitude and longitude fields. Latitude and longitude can be omitted, or obtained from an address using a geocoding service. 

Examples of current data sources for MykoMaps are:
- CSV files
- LimeSurvey questionnaires
- Google Sheets
- Nextcloud/OnlyOffice sheets
- Airtable

The data collection for Land Explorer is currently only the Land Registry data - this is converted into GeoJSON for Land Explorer to consume.

#### GitHub
Here on Github itself we have a long list of projects. Notably:

- MykoMap - an embeddable map of points with attributes, searchable and filterable - *Typescript/D3/Leaflet*
- open-data - a collection of data collection scripts constituting the "Sausage Machine" (see below) - *Ruby/Shell*
- map-sse - ESSGLOBAL SKOS vocabs, basically a taxonomy system for Co-ops and the Solidarity Economy, using semantic web data standards - *RDF data/TTL data/Shell/Ruby*
- lod.coop - permanent URL redirection service, see below - *Shell/HTML/PHP/Apache htaccess files*
- Land Explorer - a mapping application for exploring and annotating land usage
  -  front end - *React/Typescript/Mapbox*
  -  back end - *Typescript/Postgres* (was PHP/.Net/SQLServer)
- TWINE - app for managing teams of volunteers - *Typescript/React/Postgres*
- technical-infrastructure
   - *Ansible* scripts for deploying dev-1/prod-1 (see below)
- password-store - GPG encrypted `pass` secrets for Ansible, mostly
- goth - experimental RDF documentation library - *Ruby*
- shipshape - Mykomap config interrogation service - *Javascript/Gatsby/PHP*
- hookrunner - minimal build service web server - *Javascript/PHP/Shell*

#### On WebArchitects
- email/calendaring server, which handles DCC mail.

#### A Hetzner account
We have a Hetzner server running Cloudron on Ubuntu, at https://my.digitalcommons.coop/

This currently runs VMs for:
- Hosting LandExplorer
- Hosting a [Cloudron](https://cloudron.io) server.

The Cloudron server provides us with:
- The website for https://digitalcommons.coop which runs on Ghost.
- A Matrix server, with an Element front end service.
- A NextCloud server (although most docs are on Google)
- A OnlyOffice server for NextCloud
- An Uptime Kuma service monitor
- An Umami website analytics server
- Backups for the above.
- Assorted other apps which we are evaluating or don't yet use very seriously.

#### A BitWarden account
At https://bitwarden.com - used as a people-friendly password store

#### A Landex Cloudways server
Hosting for the old Land Explorer land registry borders microservice (to be retired soon.)

#### Google Apps
A Google Apps account which we use for sharing files and documents.

#### Limesurvey
A LimeSurvey account on which the questionnaires gathering information for several of the earliest MykoMap sites. This is a source of initiatives data for the "Sausage Machine"

### SEA 
We are still using some services hosted by our founders Solidarity Economy Association.

#### On WebArchitects
 - wp-1 and wp-2 - wordpress installatons on the slidarityeconomy.coop domain
 - A zonefile service for the solidarityeconomy.coop domain

#### Scaleway
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
    -  the subdomains for currently hosted MykoMaps, with dev/prod prefix as appropriate (prod prefix optional):
      - maps - more map sites which don't warrant their own domains
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
  - A cron job which runs the "Sausage Factory" AKA the "Data Factory".
    - This is a Ruby-based data collection and processing service.
    - External solidarity initiative data sources are polled for new data.
    - New data is downloaded and converted into a standard CSV format.
    - Addresses are geocoded to get location data.
    - This is then converted into RDF/TTL using the ESSGLOBAL vocabulary and others
    - The resulting static files is used to populate the `data` domains above.
    - The resulting RDF is imported into Virtuoso to populate the triplestores.

