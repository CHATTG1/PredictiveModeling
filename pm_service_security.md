---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-21"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}

# Securing your data 

Client data security is paramount. The following information outlines some of the ways that client data is protected when using the {{site.data.keyword.pm_full}} service and what you are expected to do to help in these efforts.

## Content and Data Protection

The Data Processing and Protection data sheet (Data Sheet) provides information specific to the Cloud Service regarding the type of Content enabled to be processed, the processing activities involved, the data protection features, and specifics on retention and return of Content. Any details or clarifications and terms, including Client responsibilities, around use of the Cloud Service and data protection features, if any, are set forth in this section. There may be more than one Data Sheet applicable to Client's use of the Cloud Service based upon options selected by Client. The Data Sheet may only be available in English and not available in local language. Despite any practices of local law or custom, the parties agree that they understand English and it is an appropriate language regarding acquisition and use of the Cloud Services. The following Data Sheet(s) apply to the Cloud Service and its available options. Client
acknowledges that i) IBM may modify Data Sheet(s) from time to time at IBM's sole discretion and ii) such modifications will supersede prior versions. The intent of any modification to Data Sheet(s) will be to 

1. improve or clarify existing commitments, 
2. maintain alignment to current adopted standards and applicable laws, or 
3. provide additional commitments. No modification to Data Sheet(s) will materially degrade the data protection of a Cloud Service. 

To view the Data Sheet, see [this online document](https://www.ibm.com/software/reports/compatibility/clarityreports/report/html/softwareReqsForProduct?deliverableId=33C9B7D0BF3111E7A229E0F52AF6E722).

Client is responsible to take necessary actions to order, enable, or use available data protection features for a Cloud Service and accepts responsibility for use of the Cloud Services if Client fails to take such actions, including meeting any data protection or other legal requirements regarding Content. [IBM's Data Processing Addendum](http://ibm.com/dpa) (DPA) and DPA Exhibit(s) apply and are
referenced in as part of the Agreement, if and to the extent the European General Data Protection Regulation (EU/2016/679) (GDPR) applies to personal data contained in Content. The applicable Data
Sheet(s) for this Cloud Service will serve as the DPA Exhibit(s). If the DPA applies, IBM's obligation to provide notice of changes to Subprocessors and Client's right to object to such changes will apply as set out in DPA.

## GDPR statement that applies to deep learning log files

**Disclaimer**: For ease of use and better retrievability, the deep learning training process writes to the training log files by using Compose and retrieves log file data by using Elasticsearch. Because the log data has the potential to be retrieved by a broad group of users, including but not limited to IBM customer support personnel you must not use the log files to store any personally identifiable information (PII).