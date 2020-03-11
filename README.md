# Digital Preservation using Islandora 8

## Overview

As a digital assets management platform (DAMS), Islandora provides a wide variety of functionality, from ingest to storage to management to presentation. While ingest is a prerequisite for storage, and presentation is prerequisite for access, Islandora offers a number of digital preservation capabilites that add value to its storage and management functions and that distinguish it from other digital asset management platforms. Those tools include:

* Fedora repository
   * The Fedora repository provides the storage layer for Islandora, and Islandora leverages a number of Fedora's features to provide digital preseration services.
* Islandora FITS
   * This module uses the FITS characterization tool to derive technical and format information from Islandora media.
* Islandora Riprap
   * This module, plus the Riprap microservice, enable periodic fixity checking on Islandora media.
* Islandora Bagger
   * This tool, plus the Islandora Bagger Integration module, can produce Bags for Islandora objects and media.
* Islandora PREMIS
    * This module can combine data from a number of sources (including Ripap and FITS) and provide that data such that it conforms with the PREMIS preservation metadata standard.
* Media Formats Reports
   * Provides a graphical breakdown of the media types (MIME type or PRONOM PUID) in use in an Islandora repository.

These features not only add value to Islandora's storage and managment functionality, they enable it to integrate with other systems that specialize in digital preservation. For example, it is possible to integrate Islandora and Archivematica. One type of integration is to have Islandora generate Bags that are then used to [transfer the content into Archivematica](https://www.archivematica.org/en/docs/archivematica-1.10/user-manual/transfer/bags/#bags) for further processing.

## Islandora and Digital Preservation Policies

In addition to supporting robust digital asset mananagment, Islandora's digital preservation features can be used to implement technical aspects of institutional digital preservation policies. "Policy" in this context is a formal statement of what an institution is doing to ensure long-term access to its digital assets. "Digital preservation" is the set of actions applied to digital assets to implement a policy. Your digital preservation policy says what you are doing (often in the form of "action plans"), and Islandora's digital preservation features enable you to do what you say you are doing. York University's [digital preservation policy](https://digital.library.yorku.ca/tags/digital-preservation-policy) and [action plans](https://digital.library.yorku.ca/tags/preservation-action-plan).

> Notice the above phrase "technical aspects of institutional digital preservation policies". Digital preservation is not primarily a technical task. It is based on institutional committment, and will not work in the absence of institutional committment to make digital assets accessible in the future. Simply turning on Islandora's digital presrvation features does not mean an institution is doing digital preservation. The same can be said for an platform that supports digital preservation - just turning it on it not enough. The technical tools Islandora (or Archivematica for that matter) provides enable institutions to implement their digital preservation policies and action plans. That's all they do.

## A Closer Look at Islandora's Digital Preservation Features

### Fedora repository

Fedora Repositor provides a robust storage layer for Islandora. While Drupal also provides its own storage capabilities in its public and private filesystems, and also provides integration with a wide variety of external storage platforms such as Amazon and Azure, most Islandora repositories will want to use Fedora to store original files and any other files of enduring value. Easily reproduced dertivatives, such as thumbnails, are probably not worth storing in Fedora, but what media get stored in Drupal and what media get stored in Fedora is configurable by Islandora administrators.

Fedora also provides additional services that support Islandora's digital preservation capabilities, such as checksum generation, which is used by Islandora Riprap (more below).

### Islandora FITS

[FITS](https://projects.iq.harvard.edu/fits/home) (the File Infomation Tool Set) identifies, validates and extracts technical metadata for a wide range of file formats. Many digital preservation activities depend on the information generated by FITS, for example, during format migrations. But, FITS' ability to validate a file according to a formal file format specification is extremely important, since invalid files may not render properly in user-facing software, and might not be convertable to other formats now and in the future.

FITS provides a general framework for integrating a number of specialized tools. These tools include the DROID format validation tool (which validates files against known format signatures) and the ExifTool technical metadata extraction tool. The Islandora FITS module persists the output from FITS as a Drupal media and also provides a simplified view of this information for easy inspection:

![Media fixity check status](docs/images/fits_output.png)


### Islandora Riprap

Riprap is a general fixity validation tool that is easy to integrate with Islandora repositories via the Islandora Riprap module. Riprap periodically validates the fixity of media, but the Drupal module provides users with reports that allow them to monitor fixity information at the object level:

![Media fixity check status](docs/images/islandora_riprap_details.png)

and at the repository level:

![Fixity check failures report](docs/images/fixity_events_report_failures.png)

You do not want to see a chart with so many fixity check failures! Ideally, Riprap would not detect any fixity check failures. If that is the case, Islandora Riprap will tell you your storage is working as intended:

![Fixity check failures report](docs/images/fixity_events_report_no_failures.png)

### Islandora Bagger

Islandora Bagger, as its name suggests, generates [Bags](https://en.wikipedia.org/wiki/BagIt) for Islandora objects. It is a command-line tool suitable for integration in preservation workflows, but an accompanying Drupal module provides end users with the ability to create Bags on demand and also for administrative users to create Drupal Contexts which generate Bags in response to system events. A typical Bag for an Islandora object could look like this (depending on the options specified when the Bag was created):

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
│   ├── metadata
│   │   └── MODS.xml
│   ├── metadata.csv
│   ├── media_use_summary.tsv
│   └── node.turtle.rdf
├── manifest-sha1.txt
└── tagmanifest-sha1.txt
```

### Islandora PREMIS



### Media Formats Reports

An important aspect of repository management is being able to determine what types of files are in a repository. This information is invaluable when determining how many files in the repository are in formats that are at some risk (e.g., the formats are becoming less commonly used and are therefore at risk of becoming obsolete), or when plannig for format migrations. The Media Formats Reports module provides this information using multiple definitions of "format", specifically, [MIME type](https://en.wikipedia.org/wiki/Media_type) and [PRONOM PUID](https://en.wikipedia.org/wiki/PRONOM). It presents this information as a pie chart that allows repository administrators to see at a glance how many files of each format exist in their repositories:


![Media formats report](docs/images/media_report.png)
