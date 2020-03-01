# Digital Preservation using Islandora 8

## Overview

Islandora provides a number of features that can be used to implement digital preservation policies. "Policy" in this context is a formal statement of what an institution is doing to ensure long-term access to its digital assets. "Digital preservation" is the set of actions applied to digital assets to implement a policy. Your digital preservation policy says what you are doing (often in the form of "action plans"), and Islandora's digital preservation features enable you to do what you say you are doing. York University's [digital preservation policy](https://digital.library.yorku.ca/tags/digital-preservation-policy) and [action plans](https://digital.library.yorku.ca/tags/preservation-action-plan).

Islandora is not a complete digital preservation solution. (Some would argue that "complete digital preservation solutions" do not exist.) Rather, Islandora provides tools that enable specific aspects of digital preservation. Those tools include:

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

Islandora can work with other systems that support digital preservation. For example, it is possible to integrate Islandora and Archivematica. One type of integration is to have Islandora generate Bags that are then used to [transfer the content into Archivematica](https://www.archivematica.org/en/docs/archivematica-1.10/user-manual/transfer/bags/#bags) for further processing.
