# Digital Preservation Using Islandora 8

## Overview

As a digital asset management system (DAMS), Islandora provides a wide range of functionality, from ingest to storage to management to presentation. Islandora offers a number of digital preservation features that add value to its storage and management functions in particular. Those features include:

| Feature  | Functionality                         |
--------------- |------------------------------------                           |
| Fedora repository | Provides a robust storage layer for Islandora. |
| Islandora FITS | Uses the FITS characterization tool to derive technical and format information from Islandora media.
| Islandora Riprap | Enables periodic fixity checking on Islandora media using the Riprap fixity validation service.
| Islandora Bagger | Generates Bags for Islandora objects and media. The accompanying Islandora Bagger Integration module provides additional functionality. |
| Islandora PREMIS | Combines data from a number of sources (including Ripap and FITS) such that it conforms with the PREMIS preservation metadata standard. |
| Islandora Repository Reports | Provides a graphical breakdown of the media types (MIME type or PRONOM PUID) in use in an Islandora repository, plus some other useful reports. |

These features not only add value to Islandora's storage and managment functionality, they enable Islandora to integrate with other systems that specialize in digital preservation. For example, it is possible to:

* integrate Islandora and Archivematica by having Islandora generate Bags that are then used to [transfer the content into Archivematica](https://www.archivematica.org/en/docs/archivematica-1.10/user-manual/transfer/bags/#bags) for further processing.
* generate Bags for Islandora objects that are then moved to distributed preservation storage services such as DuraCloud or WestVault. These Bags can include a record (using PREMIS) of all of the fixity checks performed on media associated with objects and can contain structural metadata linking each object to its parent object.

The only major digital preservation functionality that Islandora does not currently deliver is normalization to preservation-friendly file formats. But, as the "Normalization" section at the end of this guide explains, adding that feature to Islandora is possible.

## Islandora and Digital Preservation Policies

In addition to supporting robust digital asset mananagment, Islandora's digital preservation features can be used to implement technical aspects of institutional digital preservation policies. "Policy" in this context is a formal statement of what an institution is doing to ensure enduring access to its digital assets. Digital preservation strategy comes into play at an even higher level, but policies are where an institution states what it is doing to safeguard future access to its digital assets of enduring value. Essentially, your digital preservation policy says what you are doing (often in the form of process-oriented "action plans"), and Islandora's digital preservation features enable you to do what you say you are doing. York University's [digital preservation policy](https://digital.library.yorku.ca/tags/digital-preservation-policy) and [action plans](https://digital.library.yorku.ca/tags/preservation-action-plan) are an excellent example to emulate.

Digital preservation's relationship to standard backups is that digital preservation relies on backups but does not replace them. Conversely, backups cannot replace digital preservation. Digital preservation adds value to backups. As support for disaster recover, standard backups tend to focus on short-term access to files (generally in 1 week, 1 month, 3 month, and 1 year periods, or some variation of those time spans). Digital preservation, on the other hand, takes a much longer perspective. If you want to ensure that your digital assets are accessible in 50 years, standard backups alone are not going to be enough. Digital preservation also concerns itself with more than just "files", since it strives to ensure the integrity of the descriptive, rights, and structural metadata necessary to make files accessible and useful over the long term.

### Read This, It's Important!

Notice the phrase in the first paragraph in this section "technical aspects of institutional digital preservation policies". Digital preservation's effectiveness is directly proportional to institutional committment, and will not work in the absence of active committment to make an institution's digital assets accessible in the future. Simply turning on Islandora's digital presrvation features does not mean an institution is doing digital preservation. The same can be said for any platform or tool (open source or commercial) that supports digital preservation - just turning it on it not enough. The technical capabilities Islandora (or Archivematica for that matter) provides only *enable* institutions to implement their digital preservation policies and action plans. That's all they do. It is up to institutions to ensure (via adequate staffing levels, financial planning, and effective collaborations with other institutions) that every aspect of their policies, not only the technical aspects, are accounted for and supported.

## A Closer Look at Islandora's Digital Preservation Features

### Fedora

[Fedora](https://duraspace.org/fedora/) provides a robust storage layer for Islandora. While Drupal also provides its own storage capabilities in its public and private filesystems, and also provides integration with a wide variety of external storage platforms such as cloud providers, most Islandora repositories will want to use Fedora to store original files and any other files of enduring value. Easily reproduced dertivatives, such as thumbnails, are probably not worth storing in Fedora, but exactly what media get stored in Drupal and what media get stored in Fedora is configurable by Islandora administrators.

Fedora also provides additional services that support Islandora's digital preservation capabilities, such as checksum generation, which is used by Islandora Riprap (more on that below).

### Islandora FITS

[FITS](https://projects.iq.harvard.edu/fits/home) (the File Infomation Tool Set) identifies, validates and extracts technical metadata for a wide range of file formats. Many digital preservation activities depend on the information generated by FITS, for example, during format migrations. But, FITS' ability to validate a file according to a formal file format specification is extremely important, since invalid files may not render properly in user-facing software, and might not be convertable to other formats now and in the future.

FITS provides a framework for integrating a number of specialized tools. These tools include the DROID format validation tool (which validates files against known format signatures) and the ExifTool technical metadata extraction tool. The [Islandora FITS](https://github.com/roblib/islandora_fits) module persists the output from FITS as a Drupal media and also provides a simplified view of this information for easy inspection:

![Media fixity check status](docs/images/fits_output.png)


### Islandora Riprap

[Riprap](https://github.com/mjordan/riprap) is a general fixity validation tool that is not specific to Islandora but is easy to integrate with Islandora repositories via the [Islandora Riprap](https://github.com/mjordan/islandora_riprap) module. Riprap periodically validates the fixity of media, and the Drupal module provides users with reports that allow them to monitor fixity information at the object level:

![Media fixity check status](docs/images/islandora_riprap_details.png)

and at the repository level:

![Fixity check failures report](docs/images/fixity_events_report_failures.png)

You do not want to see a chart with so many fixity check failures! Ideally, Riprap would not detect any fixity check failures. If that is the case, Islandora Riprap will tell you your storage is working as intended:

![Fixity check failures report](docs/images/fixity_events_report_no_failures.png)

Sudden (or grandual) increases in the number of fixity event failures, or repeated failures on the same file, are a sign that your storage layer is experiencing difficulties that need to be investigated.

### Islandora Bagger

[Islandora Bagger](https://github.com/mjordan/islandora_bagger), as its name suggests, generates [Bags](https://en.wikipedia.org/wiki/BagIt) for Islandora objects. It is a command-line tool suitable for integration in automated or batch preservation workflows, but an accompanying [Drupal module](https://github.com/mjordan/islandora_bagger_integration) provides end users with the ability to create Bags on demand and also for administrative users to create Drupal Contexts which generate Bags in response to system events. A typical Bag for an Islandora object could look like this (depending on the options specified when the Bag was created):

```
/output/112
├── bag-info.txt
├── bagit.txt
├── data
│   ├── IMG_1410.JPG
│   ├── media.json
│   ├── media.jsonld
│   ├── node.json
│   ├── node.jsonld
│   ├── metadata.csv
│   ├── media_use_summary.tsv
│   └── node.turtle.rdf
├── manifest-sha1.txt
└── tagmanifest-sha1.txt
```

However, Islandora Bagger is highly configurable and custom plugins are easy to create that will add whatever metadata or files to Bags that are required by an institution's preservation action plans.

### Islandora PREMIS

[PREMIS](http://www.loc.gov/standards/premis/) is an international standard for metadata to support the preservation of digital objects and ensure their long-term usability. It documents aspects of preserved content such as an institutions' rights to preserve the content, technical attributes of the content, the outcome of fixity check events.

The [Islandora PREMIS](https://github.com/mjordan/islandora_premis) module aggregates data generated by other Islandora modules and microsrevices and serializes that data consistent with PREMIS version 3 in Turtle RDF. For example, PREMIS RDF that includes some technical metadata and some fixity check events for an object looks like this:

```
@prefix premisobject: <http://www.loc.gov/premis/rdf/v3/Object> .
@prefix premis: <http://id.loc.gov/vocabulary/preservation/eventOutcome> .
@prefix ebucore: <https://www.ebu.ch/metadata/ontologies/ebucore#> .
@prefix cryphashfunc: <http://id.loc.gov/vocabulary/preservation/cryptographicHashFunctions/> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix dc: <http://purl.org/dc/terms/> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .
@prefix schema: <http://schema.org/> .

<http://localhost:8000/node/1>
  a <http://pcdm.org/models#Object>, <https://schema.org/DigitalDocument>, <http://www.loc.gov/premis/rdf/v3/IntellectualEntity> ;
  dc:extent "1 item"^^xsd:string ;
  dc:title "An interesting document!"@en ;
  schema:author <http://localhost:8000/user/1> ;
  schema:dateCreated "2019-12-09T14:39:07+00:00"^^xsd:dateTime ;
  schema:dateModified "2019-12-09T14:41:25+00:00"^^xsd:dateTime ;
  schema:sameAs <http://localhost:8000/node/1> .

<http://127.0.0.1:8080/fcrepo/rest/2019-12/I am an interesting document.pdf>
  a premisobject:File ;
  premis:size "534450" ;
  premis:compositionLevel 0 ;
  ebucore:hasMimeType "application/pdf" ;
  premis:fixity "7b44c786-6d7e-4764-b278-d1ac1d503d99", "324bd1b9-0a38-400d-bd07-2026801e1040", "458f3e38-34c8-412d-879f-0ec3c5a1c37c", "eb7951ac-3baf-4661-a181-76ef0995744b", "d3e39c84-ae5d-44bb-b8ce-fbe9289b1f31" .

<7b44c786-6d7e-4764-b278-d1ac1d503d99>
  a <http://id.loc.gov/vocabulary/preservation/eventType/fix>, cryphashfunc:sha256 ;
  rdf:value "6933a46f55f27a62689406ea33c650b1c16d6268ee81f6c6a2a89c63aeec9d27" ;
  dc:created "2019-12-21T15:41:00-0600" ;
  premis:outcome "success" .

<http://localhost:8000/user/1> a schema:Person .
<324bd1b9-0a38-400d-bd07-2026801e1040>
  a <http://id.loc.gov/vocabulary/preservation/eventType/fix>, cryphashfunc:sha256 ;
  rdf:value "6933a46f55f27a62689406ea33c650b1c16d6268ee81f6c6a2a89c63aeec9d27" ;
  dc:created "2019-12-22T11:44:31-0600" ;
  premis:outcome "success" .

<458f3e38-34c8-412d-879f-0ec3c5a1c37c>
  a <http://id.loc.gov/vocabulary/preservation/eventType/fix>, cryphashfunc:sha256 ;
  rdf:value "6933a46f55f27a62689406ea33c650b1c16d6268ee81f6c6a2a89c63aeec9d27" ;
  dc:created "2019-12-22T16:16:37-0600" ;
  premis:outcome "success" .

<eb7951ac-3baf-4661-a181-76ef0995744b>
  a <http://id.loc.gov/vocabulary/preservation/eventType/fix>, cryphashfunc:sha256 ;
  rdf:value "6933a46f55f27a62689406ea33c650b1c16d6268ee81f6c6a2a89c63aeec9d27" ;
  dc:created "2019-12-23T11:28:13-0600" ;
  premis:outcome "success" .

<d3e39c84-ae5d-44bb-b8ce-fbe9289b1f31>
  a <http://id.loc.gov/vocabulary/preservation/eventType/fix>, cryphashfunc:sha256 ;
  rdf:value "6933a46f55f27a62689406ea33c650b1c16d6268ee81f6c6a2a89c63aeec9d27" ;
  dc:created "2019-12-23T16:19:05-0600" ;
  premis:outcome "success" .
```

Islandora PREMIS is currently the least mature of Islandora's preservation features, but as more sources of data become available, they can be integrated into Islandora's PREMIS RDF to present a more complete expression of objects' preservation lifecycle within Islandora.

### Islandora Repository Reports

An important aspect of repository management is being able to determine what types of files are in a repository. This information is invaluable when determining how many files in the repository are in formats that are at some risk (e.g., the formats are becoming less commonly used and are therefore at risk of becoming obsolete), or when planning for format migrations. The [Islandora Repository Reports](https://github.com/mjordan/islandora_repository_reports) module provides this information using multiple definitions of "format", specifically, [MIME type](https://en.wikipedia.org/wiki/Media_type) and [PRONOM PUID](https://en.wikipedia.org/wiki/PRONOM). It presents this information as a pie chart that allows repository administrators to see at a glance how many files of each format exist in their repositories:

![Media formats report](docs/images/media_report.png)

### Normalization

Islandora does not currently perform normalization of ingested files into preservation-friendly formats. It does tranform many types of files into specific formats or characteristics for specific purposes using a standard microservices architecture; for example, if a user ingests a TIFF, Islandora will create a JPEG2000 version of that TIFF for presentation to the user. To date, no one from the Islandora community has applied Islandora's microservices architecture to preservation-friendly normalization tasks. Given Islandora's use of microservices to transform content into presentation-friendly formats (e.g. TIFF to JPEG2000), it has the potential to normalize content into preservation-friendly formats in the same way as other platforms do. Islandora already incorporates many of the prerequisites for using Archivematica's [Format Policie Registry](https://www.archivematica.org/en/docs/archivematica-1.12/user-manual/preservation/preservation-planning/) or [Preservation Action Registries](https://parcore.org/) if the community defines that functionality as a priority.

## Islandora's Place in Your Institution's Digital Preservation Strategy

Insitutions that use Islandora 8 as a DAMS have a robust set of tools, but using Islandora's preservation capabilities in the absence of a digital preservation policy is not enough to ensure enduring access to an institution's digital assets. Digital preservation requires a strategy, not just a set of tools. A digital preservation strategy implements policies, and, just as importantly, institutional commitment in the form of staff and financial resources to support those policies. 
