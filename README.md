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

In addition to supporting robust digital asset mananagment, Islandora's digital preservation features can be used to implement institutional digital preservation policies. "Policy" in this context is a formal statement of what an institution is doing to ensure long-term access to its digital assets. "Digital preservation" is the set of actions applied to digital assets to implement a policy. Your digital preservation policy says what you are doing (often in the form of "action plans"), and Islandora's digital preservation features enable you to do what you say you are doing. York University's [digital preservation policy](https://digital.library.yorku.ca/tags/digital-preservation-policy) and [action plans](https://digital.library.yorku.ca/tags/preservation-action-plan).

## A Closer Look at Islandora's Digital Preservation Features

### Fedora repository

### Islandora FITS

### Islandora Riprap

![Media fixity check status]](ocs/images/islandora_riprap_details.png)

![Fixity check failures report](docs/images/fixity_events_report_failures.png)

### Islandora Bagger

### Islandora PREMIS

### Media Formats Reports


![Media formats report](docs/images/media_report.png)
