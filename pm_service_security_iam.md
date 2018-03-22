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

# Managing user access with Identity and Access Management

**This content has moved to a [new location](https://datascience.ibm.com/docs/content/analyze-data/pm_service_security_iam.html). Check there for the most up-to-date information.** 

Update any bookmarks you might have to the old location.


_____________


Access to {{site.data.keyword.pm_full}} service instances for users in your account is controlled by {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM). Every user that accesses the {{site.data.keyword.pm_full}} service in your account must be assigned an access policy with an IAM user role defined. That policy determines what actions the user can perform within the context of the service or instance you select. The allowable actions are customized and defined by the {{site.data.keyword.Bluemix_notm}} service as operations that are allowed to be performed on the service. The actions are then mapped to IAM user roles.

Policies enable access to be granted at different levels. Some of the options include the following: 

* Access across all instances of the service in your account
* Access to an individual service instance in your account
* Access to a specific resource within an instance
* Access to all IAM-enabled services in your account

After you define the scope of the access policy, you assign a role. Review the following tables which outline what actions each role allows within the {{site.data.keyword.pm_full}} service.

IAM is supported on WML REST API level with following assumptions:

1. We support the following roles:
  - **Reader** service access role - for read-only access to the {{site.data.keyword.pm_short}} repository
  - **Writer** service access role - for regular user to get access to the REST API resources
  - **Administrator** role - for privileged access to the management API (used for e.g. by service broker)
2. IAM policy can be set on the account / region / service instance level. 

## Learn more

For information about assigning user roles in the UI, see [Managing IAM access](/docs/iam/mngiam.html#iammanidaccser).