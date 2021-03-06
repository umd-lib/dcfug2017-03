title: Introducing Archelon
author:
  name: Peter Eichman, Josh Westgard
  email: peichman@umd.edu, westgard@umd.edu
  url: https://github.com/umd-lib/dcfug2017-03
theme: theme
output: index.html

--

# Introducing Archelon
## UMD Libraries Fedora 4 Administrative Tool

--

### Archelon 1.0 has Reached <del>the Ocean</del> Production!

![](LeatherbacksCrawlingToTheSea.jpg)  
<small class="photo-credit">U.S. Fish and Wildlife Service Southeast Region / <a href="http://creativecommons.org/licenses/by/2.0">CC-BY-2.0</a></small>

--

### Archelon?

![](ArchelonTurtle.jpg)
<br/><small class="photo-credit">Dmitry Bogdanov / <a href="http://creativecommons.org/licenses/by/3.0">CC-BY-3.0</a></small>

--

### Archelon?

* Named for an ancient genus of giant sea turtles
* Name means “ruler turtle” in Greek.
* Lived ~80.5 million years ago in North America
* Largest ever recorded species of turtle by size
* Second largest by weight
* <https://en.wikipedia.org/wiki/Archelon>

--

### Screenshot: Home Page

![](ArchelonHomeScreen.png)

--

### Screenshot: Search Results

![](ArchelonSearchResults.png)

--

### Screenshot: Details Page

![](ArchelonDetailsPageTop.png)

--

### Screenshot: Details Page, Cont'd

![](ArchelonDetailsPageBottom.png)

--

### The Stack

* Fedora 4.7.0
* Solr 6
* Loris IIIF Image API Server
* PCDM Manifests IIIF Presentation API Server
* Mirador IIIF Viewer

--

### Solr 6: Subqueries

* Index the `pcdm:hasMember` field:

  ```
pcdm_members = pcdm:hasMember :: xsd:string
  ```
* Use subquery to get title at query time:

  ```xml
<str name="fl">*,pcdm_members_info:[subquery]</str>
<str name="pcdm_members_info.q">{!terms f=id v=$row.pcdm_members}</str>
<str name="pcdm_members_info.fl">id,title,component</str>
<str name="pcdm_members_info.rows">100</str>
  ```

--

### PCDM Manifests

* In-house developed Rails app
* (Partial) implementation of the IIIF Presentation API
* Converts RDF metadata modelled in PCDM to JSON format defined by the IIIF Presentation API
* Serves IIIF manifests for newspaper issues
* Redirects requests for pages to their corresponding issue
* <https://github.com/umd-lib/pcdm-manifests>

--

### Performance Challenges

* PCDM Manifests initially talked directly to Fedora 4
* Lots of requests
* Much...too...slow

--

### Solution

* Index all metadata needed to generate manifests in Solr
* Talk to Solr instead of Fedora 4 directly
* Single request returns all info needed
* Much faster!

--

### Loris Image Server

* Using version 2.1.0
* Contibuted code back to the Loris community
* <https://github.com/loris-imageserver/loris>

--

### Loris to Fedora Connection

* Using the templated HTTP resolver
* Maps prefixed CURIE-style IIIF IDs:
  ```
  fcrepo:pcdm%2Fd3%2F05%2Fa8%2F4f%2Fd305a84f-6287-439c-b20d-b3d19e05aa44
  ```
* To Fedora 4 URIs:
  ```
  https://fcrepo.lib.umd.edu/fcrepo/rest/pcdm/d3/05/a8/4f/d305a84f-6287-439c-b20d-b3d19e05aa44
  ```
* Images are cached by Loris after first retrieval from Fedora

--

### Mirador IIIF Image Viewer

* Static wrapper around a forked build of Mirador
* Embeddable via `<iframe>`
* Some custom code contributed back to community
* Some changes are unique to UMD
* <https://github.com/umd-lib/mirador-static>
* <https://github.com/umd-lib/mirador>

--

### Mirador Customizations Presentation

* Code4Lib 2017 Poster Presentation
* “Newspapers Re-Issued: Developing a custom IIIF newspaper viewer at the University of Maryland Libraries”
* <http://drum.lib.umd.edu/handle/1903/19172>

--

### Putting it All Together

![](IIIFEcosystem.png)

--

### What’s Next?

![](SingleLeatherback.jpg)  
<small class="photo-credit">U.S. Fish and Wildlife Service Southeast Region / <a href="http://creativecommons.org/licenses/by/2.0">CC-BY-2.0</a></small>

--

### Planned Features for 1.1

* OCR text search
* OCR hit highlighting
* Article segmentation
* Implemented with Web Annotations
* <https://www.w3.org/TR/annotation-vocab/>

--

### Beyond 1.x: Writing to the Repo

* Complete management of digital objects
* Hydra-based (?) (or now: Agora-Repository-based?) 
* Access control management
* Batch-loader integration

--

### Repository Goals

* Continue loading new content first
* Migrate Fedora 2 content to Fedora 4
* Move off of direct filesystem storage for binaries
* General-purpose cache to replace Loris' custom cache
* Retain our repository-centric model

--

### Archelon on GitHub

* <https://github.com/umd-lib/archelon>

--

### Image Links

* Leatherback Sea Turtle Hatchlings
  <br><small><https://commons.wikimedia.org/wiki/File:Leatherbacks_crawling_to_the_sea.jpg></small>
* Archelon Restoration
  <br><small><https://commons.wikimedia.org/wiki/File:Archelon1DB.jpg></small>

[CC-BY-2.0]: http://creativecommons.org/licenses/by/2.0
