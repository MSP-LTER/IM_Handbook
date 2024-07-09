---
# MSP LTER Information Management Handbook (WIP)
### Information Manager: Mary Marek-Spartz
### Program and Communications Manager: Meredith Keller
---

<sup> ~ *special thanks to the very cool people at BLE LTER for sharing their guide as a template!* ~</sup>



<a id="header"></a>

- [Introduction to MSP LTER Information Management](#introduction-to-msp-lter-information-management)
	

- [Tools we use](#tools-we-use)
	- [Languages](#languages)
	- [Software](#software-and-tools)
- [Tools we develop and maintain](#tools-we-develop-and-maintain)
	- [RStudio Server](#rstudio-server)
	- [R Shiny apps](#r-shiny)
- [Getting access to things](#getting-access-to-things)
- [Publishing data and metadata](#publishing-data-and-metadata)
 	- [Tabular](#tabular-data)
 	- [Spatial](#spatial-data)
 	- [Other data](#other-data)
- [Striving for C.A.R.E., not just F.A.I.R.](#striving-for-care)
	

<!-- /MarkdownTOC -->


<a id="introduction-to-msp-lter-information-management"></a>

## Introduction to MSP LTER Information Management

The Minneapolis-St. Paul Urban LTER (MSP LTER) joined the Long Term Ecological Research Network in 2021. Our site spans the seven-county MSP Metropolitan Area, one of the most spatially distributed population centers in the country (in terms of humans per square mile). This means that MSP researchers collect and compile data from a large amount of spatially diverse governing bodies: ~117 cities (including the "Twin Cities" Minneapolis and St. Paul), 33 Watershed Management Organizations, and numerous park districts and non-profit entities. 

Our research sites can range from a sample from an individual lawn, to a policy that applies to an entire municipality, to an ecological feature that crosses county boundaries. Consequently, Information Management at the MSP LTER frequently incorporates spatial data, social science data, and other non-tabular forms of information along with tabular datasets in our research activities. 

Mary Marek-Spartz (She/Her) was recruited as the MSP LTER Information Manager in the summer of 2021. 



<a id="tools-we-use"></a>
## Tools we use

<a id="languages"></a>
### Languages

Many MSPers use R/RStudio and its suite of packages, especially Rspatial packages. In a survey of data management and analysis tools given to MSP researchers in late 2021 (the majority of whom were postdocs and grad students), 13/15 respondents indicated they used R, making it by far the most consistently used tool across MSP. Accordingly, at the onset of our site's infrastructure development, Mary endeavored to build many scripts and tools in R to promote trust, collaboration, interoperability, reproducibility, and overall sanity among us. 

That said, other languages used by MSPers include Python, Arduino, MATLAB, BASH, Git, HTML, CSS, JavaScript, and probably more! 

Our metadata languages include variants of XML - Ecological Metadata Language (EML), as well as The [Minnesota Geographic Metadata Standards (MGMG)](https://www.mngeo.state.mn.us/committee/standards/mgmg/metadata.htm) based on the [Federal Content Standard for Digital Geospatial Metadata](http://www.fgdc.gov/metadata/geospatial-metadata-standards). 

<a id="software-and-tools"></a>
### Software

We strive to use Free and Open Source Software (FOSS) whenever it is possible. This includes our GIS workflows! Some use QGIS, but many of us use R as our primary GIS. 

- **Versioning:** Most research teams are using Google Drive documents to collaboratively edit, but some use Git, mainly through the Git interface in RStudio, directly on GitHub in the browser, or through a Git client. The MSP LTER started its own [GitHub](https://github.com/MSP-LTER/) at the beginning of 2024, and we are in the process of adding MSPers already on GitHub to the organization and adding useful scripts and applications to its repositories.

- **GIS software**
	- 	R / RStudio with R Spatial packages 
		- Important R Spatial packages are retiring in 2023 - rgdal, rgeos, raster. Mary has migrated most active scripts to `sf` and `terra`, however, our [RStudio server](#tools-we-develop-and-maintain) is equiped with retired packages as well as the new ones to serve as a sort of mirror of CRAN R Spatial packages pre-2023.
	-  ArcGIS and Esri Online 
	   - Very much not FOSS, but the University of Minnesota has an organizational license to ESRI products that can be administered by U Spatial
	-  QGIS
		- FOSS and MacOS compatible, around one third of grad students and post docs at the MSP LTER were using this when last surveyed, but anecdotally, that number has gone up. 
	-  Survey123 
	   - another ESRI product that requires a license 
	-  Kobo Toolbox 
	   - Freemium model
	-  [Particle Cloud](https://www.particle.io)
		- IoT connectivity management software for remote devices (sap flux sensors)

- **Analysis Software**
	- R/RStudio (Posit)
	- Excel
	- Python
	- Atlas.ti
	- MATLAB
	- JMP
	- EnVivo
	- Stata

- **File sharing and backup**
	- Virtually everyone uses Google Drive and many work directly from the shared MSP LTER Google Drive.
Mary uses `rclone` to backup the shared drive once a month from remote to an AWS S3 bucket. Mary referenced [this tutorial](https://dev.to/gerkibz/moving-data-from-google-drive-to-aws-s3-bucket-27hn) to set it up. Then once it is configured and authenticated, run this terminal script monthly:
		```rclone sync --interactive google-drive-lter: s3-lter:msplter-googledrive-backup ```
		
	- Other file sharing services individual researchers at the MSP LTER use:
		- University Departmental shared file server (access is usually on a department-by-department basis, and MSP is spread out over multiple departments and colleges within the University)
		- Dropbox
		- Box: UMN's Box Secure Storage that is compliant with its IRB standards for storing private, restricted data. This is especially used by those storing interview data.
		- Esri Geodtabase
	
Data housed on Mary's computer additionally gets backed up on-site to a Synology server and remotely to iCloud.

<a id="tools-we-develop-and-maintain"></a>
## Tools we develop and maintain

<a id="rstudio-server"></a>
### The MSP LTER RStudio Server

The MSP LTER frequently has GIS workflows that are too taxing for an individual's laptop, but too time sensitive or not large enough to justify waiting in line for Supercomputing Institute resources. We use Amazon Web Services to fill this gap. Marek-Spartz worked with Tom Kautz in CBS IT and the UofMN Cloud Services team to get access (login: [z.umn.edu/AWS](https://z.umn.edu/AWS)) to an AWS account that was configured to bill MSP LTER IM budget string, with a custom domain at [https://msplterstudio.org/](https://msplterstudio.org/). An AWS lightsail 8GB instance was installed with the open-source distribution of RStudio server, along with all the unit and geospatial libraries needed for RSpatial packages, including versions of `GDAL`, `proj`, and `units` that are backwards compatible with retiring RSpatial packages. 8GB appears to be the minimum required to use R as a GIS with this solution, costing around `$35` a month to run, but a snapshot of the configuration is saved and can be fired up on a more powerful AWS machine for a shorter duration of time for researchers to run more memory-intense R processes. 

A description about how to set up the [MSP RStudio server](https://msplterstudio.org) is documented as a DataBits article. 

The server is paused during less busy times to save money.

Eric Straavaldsen (straa007@umn.edu) with the UMN Cloud Services team can be contacted for ongoing support around AWS services at the University of Minnesota.


<a id="r-shiny"></a>
### R Shiny Apps

1. TCMA public tree inventory and climate vulnerability report: [https://marymarek-spartz.shinyapps.io/TreeInventoryReport/](https://marymarek-spartz.shinyapps.io/TreeInventoryReport/)

2. Zotero-enabled searchable publication catalog

	Research publications: [https://alfalimajuliett.shinyapps.io/	msp_lter_catalog/](https://alfalimajuliett.shinyapps.io/msp_lter_catalog/)
 
	Data publications: [https://alfalimajuliett.shinyapps.io/Datasets/](https://alfalimajuliett.shinyapps.io/Datasets/)
	
3. Meteorological data portal
[https://alfalimajuliett.shinyapps.io/TCMA_weather_data/](https://alfalimajuliett.shinyapps.io/TCMA_weather_data/)
4. [Stew Map](https://mspurbanlter.umn.edu/community-engaged-research/stew-map) placeholder for Stew Map data that is just now being compiled
5. [Urban Tree Canopy Data Nugget](https://alfalimajuliett.shinyapps.io/treeCanopy/)

 

<a id="getting-access-to-things"></a>
## Getting access to things

Many logins are connected to the IM's personal UMN credentials. Tools and resources that will require configuration with UMN login credentials and DUO Authentication are denoted with a `*`. Access that can be granted by our site PI is denoted with `+`.

New members of the MSP LTER IM team will need access to:

- The departmental account email address msplterdata@umn.edu `*` `+` It is possible it will be easier to create a new account, as the departmental accounts expire every year and are tied to the credentials of those who initiated them.
- Access to Public and Private MSP shared Google Drive `+`
- Contact U of MN Cloud Services to set up access to AWS account (login: [z.umn.edu/AWS](z.umn.edu/AWS)) `*`
- Collaborator to the MSP-LTER Github organization
- Editor on Zotero group library
- EDI data portal login: production and staging (one login for all) to upload packages under the "knb-lter-msp" scope. 
- Be added to the `msp_lter_all` mailing list `+`
- Contact ucm@umn.edu to be added as and a site administrator to the Drupal Lite website, then you can login with your UMN internet ID at [https://mspurbanlter.umn.edu/saml_login](https://mspurbanlter.umn.edu/saml_login)  `*`

Meredith Keller has the Twitter credentials and access to the MailChimp account for publishing the City Buzz newsletter.


<a id="publishing-data-and-metadata"></a>
## Publishing data and metadata
For completed data products, there is a hierarchy for determining the publication and archiving venue based on the dataset type and intended audience.

<a id="tabular-data"></a> 

### Tabular Data
-  The [**Environmental Data Initiative**](https://edirepository.org) is first priority for tabular data
 - Researchers are asked to complete a [metadata template](https://docs.google.com/spreadsheets/d/1wth-xTjJDmfV7zBVlFQ_cgRjhS7jLE0YbCzCZdPQ6I0/edit#gid=376636379) (available for download on our website) designed to collect the necessary elements to populate an EML document. 
 - Researchers may aid in filling out the EML document more directly by collaborating via the [ezEML](https://ezeml.edirepository.org) browser tool. 

<a id="spatial-data"></a>

### Spatial Data
- May also be archived on **EDI**
	- similar process as above, with additional input from the Information Manager to fill out `<spatialRaster>` and `<spatialVector>` elements. 
- If the spatial data are regionally specific or are extended or derived from datasets from the [**Minnesota Geospatial Commons**](https://gisdata.mn.gov), we may opt to archive with the Commons, to ensure data is reaching the greatest number of potential users.
  	- Metadata for the Commons are prepared following the Minnesota Geospatial Metadata Guidelines, a streamlined version of the Federal Content Standard for Digital Geospatial Metadata.
 	- Mary uses the [Minnesota Metadata Editor](https://gisdata.mn.gov/content/?q=help/mn_metadata_editor) application (NOTE: only available for Windows T_T)

<a id="other-data"></a>
### Other Documents and IRB-protected data
- Other forms of data including interviews, documents that have been digitized and coded, and IRB-protected data are planned for the Qualitative Data Repository, although these datasets are still being collected and analyzed. 
- During data collection, the IM typically keeps copies of the IRB data management plan for a project, but does not have access to the de-identified data from the project

<a id="striving-for-care"></a>

## Striving for C.A.R.E., not just F.A.I.R.

Although MSP prioritizes discoverability and accessibility of data, in some cases data products remain or are temporarily disaggregated from national data platforms. This data may include knowledge and samples collected with Indigenous communities, or other non-governmental partner organizations, where it becomes important to preserve the local intentionality of the data. In these situations, data products are in the form that best serves the partner organization, such as internal reports, and any publication to a broader data platform must be predicated on an MOU or other co-production agreements between all parties involved in creating the dataset. 

The Information Management team is necessarily and directly involved with the Community Engagement team, the MSP LTER DEIJ committee, and with community partners to develop and align our goals around data sovereignty and co-production.  
