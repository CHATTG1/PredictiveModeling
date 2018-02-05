---

copyright:
  years: 2016, 2018
lastupdated: "2018-02-05"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Machine Learning service batch job API for IBM SPSS Modeler models

The batch job API for the {{site.data.keyword.pm_full}} service supports the
long-running tasks that relate to model training, model evaluation,
and batch scoring. This API manages two asset types: the IBM® SPSS®
Modeler stream files that are used in the batch jobs and the job
definitions that are submitted. 
{: shortdesc}

For each SPSS® Modeler stream file that you upload,
there might be many jobs of many types that are submitted. If a job has the
potential to update the Modeler stream file contents, the
modified file is included in the job result attachments. In
a model evaluation job type, all evaluation files that are generated are in the job results attachments.

**Note**: The data that appears in the dashboard is related only to real-time predictions, such as the data from the upload feature.

## Deleting jobs

You can delete jobs. If a job is running, deleting a job first cancels the job. Use the `DELETE` command with the `job ID`. You can cancel more than one job at a time by passing multiple IDs.

```
DELETE https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job ID
1, job ID 2,...}?accesskey=xxxxxxxxxx
```
{: codeblock}

The return indicates how many of the jobs, which are referenced by ID in the request, are deleted. If this number does not match the list that you passed in the request, you must inspect the status of
individual jobs.

## Checking the status of a job

You can get the status of your `job ID` at any point by using the `GET` command:


```
GET https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job
ID}/status?accesskey=xxxxxxxxxx
```
{: codeblock}

The JSON file that is returned indicates the job status and, if the job completed
successfully, a data Url that you can use to obtain all generated
file content.

## Resubmit a job

To resubmit a job, use the `PUT` command with the `job ID`. To resubmit a job, the job must not be running.

If the ID you reference does not exist, the following call creates a new job. The return status is 201 versus 200 (indicating which of
these happened). You must pass the job definition JSON file to be used
in this execution.

```
PUT https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job
ID}?accesskey=xxxxxxxxxx
```
{: codeblock}

Resubmit the job with the `content_type` value set to "application/json". You must include the new or updated job definition JSON file in the body of the request.

## Submit a job against an uploaded modeler stream file

To put a job in the execution queue, use the `PUT` command:

```
PUT https://{PA Bluemix load balancer URL}/pm/v1/jobs/{job
ID}?accesskey=xxxxxxxxxx
```
{: codeblock}

Submit the job with the `content_type` value set to "application/json". You must include the new or updated job definition JSON file in the body of the request.

This request returns immediately and indicates success if the job
definition was placed in the execution queue. There is no test of
the ability to successfully execute the Modeler stream as
configured in your job definition. The job will be executed by
one of the {{site.data.keyword.pm_short}} job servers in the cloud, and you can
monitor its status. If you are performing model training and indicating
that you want an auto refresh, the job replaces the original
Modeler stream file upon successful execution of the job.

## Upload a stream file to use in your jobs

**Note**: The {{site.data.keyword.pm_short}} dashboard is only for real-time
scoring. You cannot use it for running jobs (batch scoring).

To make a Modeler stream file accessible for jobs, use the `PUT` command:

```
PUT https://{PA Bluemix load balancer URL}/pm/v1/file/{file
ID}?accesskey=xxxxxxxxxx
```
{: codeblock}

with content_type "multipart/form-data" passing the file in the
request.

The unique name used (`file ID` in a `PUT` call) is what you will
reference in a `DELETE` call to the file API as well as the model
reference in your job definitions. Note that the namespace of files
used in your jobs is totally in your control. The `PUT` command of a
file under the same `file ID` implicitly replaces the current
copy.

To generate a list of all files uploaded for your jobs, use a `GET` command, but omit the `file ID` parameter. To retrieve a specific file, use a `GET` command with the `file ID` parameter. You can also delete an uploaded file by issuing a `DELETE` command. This causes errors in any pending job execution that references the file.

Request example:

```
    Content-Type: multipart/form-data
    Parameters:
        Form parameters:
            model_file: the model file to upload
        Path parameters:
            contextId: the unique identifier to assign to your model or a reference to the deployed model to refresh
        Query Parameters:
            accesskey: access_key from env.VCAP_SERVICES
```
{: codeblock}

Response when the deployment succeeds:

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":true, 
           "message":"detailed information"  
         }      
```
{: codeblock}

Response when the deployment fails:

```
    Content-Type: application/json
    Status code: 200
    body:
        {
           "flag":false, 
           "message":"reason"
        }
```
{: codeblock}

## Job types

**Training**: This job type indicates that all "model builder"
terminal nodes should be executed in the Modeler stream. After
successful job completion, an updated Modeler stream with newly
trained model nuggets will be in the job results that can be
retrieved. If the Modeler stream file has links from the model
build nodes to the trained model nuggets in the evaluation and
scoring branches, this will cause a refresh of those nodes.

**Evaluation**: This job type triggers execution of all "document
builder" terminal nodes (mainly from the Graphs and Output tabs
in Modeler client) that generate static report file content that
can be passed back to the caller. The scoring branch is not
considered as part of this job type.

**Auto-Refresh**: A version of the `TRAINING` job type where, on
successful completion of the job, the original Modeler stream
file in the batch file list will be updated. Evaluation and
explicit decision regarding a refresh event of the Modeler
streams deployed for real-time scoring is assumed and not covered
in auto-refresh at this time.

**Batch Score**: Execution of the terminal node you have applied the
Use as Scoring Branch option, indicating this is the scoring
branch in this Modeler stream design. Job definition must specify
Export as well as Source details.

**Run Stream**: Execution is similar to clicking the green "run"
icon in Modeler with the Run this script option selected on the
Execution tab of the stream properties. Usage covers need for
scripted execution of model training or other job types. All
dynamic control of the script must be handled by stream
parameters, with parameter values passed in the job definition.

## Job status

**Pending**: The job definition has been submitted but has not been
claimed by a job server for execution yet.

**Running**: The job definition has been claimed by a job server and
is executing.

**Canceling**: The job is in the process of being canceled.

**Canceled**: The job has been canceled.

**Failed**: The job failed. Details about the cause of failure are
returned on GET on the job status.

**Success**: The job ran successfully. Any result communicated for
this event is returned in the JSON of the GET on job status.

## Job API details

`POST /v1/jobs/{id}`

Description: Submits a job definition for execution. An error
will result if the `job ID` already exists.

Content Types:

```
@Consumes({“application/json”})
@Produces({“application/json”})
```
{: codeblock}

Parameters:

Access key returned as credentials in provision or bind:

```
@QueryParam("accesskey")
```
{: codeblock}

User-specified `job ID`. Must be unique to a {{site.data.keyword.pm_short}}
service instance:

```
@PathParam("id")
```
{: codeblock}

JSON job definition (see Job definition JSON):

```
@BodyParam
```
{: codeblock}

Responses:

Success. Job is submitted for execution and returns the JSON
describing the submitted job.

```
@ApiResponse(code = 200)
```
{: codeblock}

Job already exists error. A job with this ID is already
submitted. The message returned describes this error.

```
@ApiResponse(code = 409)
```
{: codeblock}

Other error. JSON of exception returned

```
@ApiResponse(code = 500)
```
{: codeblock}

`PUT /v1/jobs/{id}`

Description: Create or update a job. If a job with this ID does
not exist, create it; otherwise, update it (which, effectively,
resubmits it for execution).

Content Types:

```
@Consumes({“application/json”})
@Produces({“application/json”})
```
{: codeblock}

Parameters:

Access key returned as credentials in provision or bind:

```
@QueryParam("accesskey")
```
{: codeblock}

User-specified `job ID`:

```
@PathParam("id")
```
{: codeblock}

JSON job definition (see Job definition JSON):

```
@BodyParam
```
{: codeblock}

Responses:

Success. Job is updated and submitted for execution. Returns the
JSON describing the submitted job.

```
@ApiResponse(code = 200)
```
{: codeblock}

Success. Job is created and submitted for execution. Returns the
JSON describing the submitted job.

```
@ApiResponse(code = 201)
```
{: codeblock}

Other error. JSON of exception returned.

```
@ApiResponse(code = 500)
```
{: codeblock}

`GET /v1/jobs`

Description: Returns a list of all defined jobs on this Machine
Learning service instance.

Content Types:

```
@Produces({“application/json”})
```
{: codeblock}

Parameters:

Access key returned as credentials in provision or bind:

```
@QueryParam("accesskey")
```
{: codeblock}

Responses:

Success. Returns the JSON array of all job definitions:

```
@ApiResponse(code = 200)
```
{: codeblock}

Other error. JSON of exception returned:

```
@ApiResponse(code = 500)
```
{: codeblock}

`GET /v1/jobs/{id}`

Description: Request the return of a specific job definition.

Content Types:

```
@Produces({“application/json”})
```
{: codeblock}

Parameters:

Access key returned as credentials in provision or bind:

```
@QueryParam("accesskey")
```
{: codeblock}

Submitted `job ID`:

```
@PathParam("id")
```
{: codeblock}

Responses:

Success. Returns the JSON describing the referenced job:

```
@ApiResponse(code = 200)
```
{: codeblock}

Job not found error. Details in message returned:

```
@ApiResponse(code = 404)
```
{: codeblock}

Other error. JSON of exception returned:

```
@ApiResponse(code = 500)
```
{: codeblock}

`GET /v1/jobs/{id}/status`

Description: Retrieve the status of a specific job.

Content Types:

```
@Produces({“application/json”})
```
{: codeblock}

Parameters:

Access key returned as credentials in provision or bind:

```
@QueryParam("accesskey")
```
{: codeblock}

Submitted `job ID`:

```
@PathParam("id")
```
{: codeblock}

Responses:

Success. Returns the JSON describing the Job status:

```
@ApiResponse(code = 200)
```
{: codeblock}

Job not found error, details in message returned:

```
@ApiResponse(code = 404)
```
{: codeblock}

Other error. JSON of exception returned:

```
@ApiResponse(code = 500)
```
{: codeblock}

`DELETE /v1/jobs/{ids}`

Description: Delete one or more jobs. Will cancel job if it is
currently running.

Content Types:

```
@Produces({“application/json”})
```
{: codeblock}

Parameters:

Access key returned as credentials in provision or bind:

```
@QueryParam("accesskey")
```
{: codeblock}

Submitted `job ID` or `job ID` list with ID values split by a comma (,)

```
@PathParam("id" or “id,id,id”)
```
{: codeblock}

Responses:

Success. Returns the number of jobs deleted:

```
@ApiResponse(code = 200)
```
{: codeblock}

Other error. JSON of exception returned:

```
@ApiResponse(code = 500)
```
{: codeblock}

## Job definition JSON

The job definition JSON contains the following general sections:

Job type and predictive model reference

**Note**: The data that is shown in the dashboard is only related to real-time predictions, including the data from the upload feature.

### Action types

**TRAINING** – Executes the model builder node training or
   refreshing model nuggets. Updated Modeler stream is retrieved
   in job results.

**EVALUATION** – Executes document builder and analysis nodes
   evaluating the trained model.

**AUTO_REFRESH** – Performs TRAINING and updates file contents in
   batch file upload space.

**BATCH_SCORE** – Executes the scoring branch and exports the
   resulting data as directed by the job definition.

**RUN_STREAM** – Executes the Modeler stream as indicated in the
   stream properties; most often used when scripted execution is
   required.

Model: ID as specified in the file upload action for the batch
job reference. For more information, see Uploading a stream file
to use in your jobs. Note that `/pm/v1/file` is used.

```
"action": "TRAINING",
"model": {
     "id": "str2" 
     "name":"stream1.str" 
},
```
{: codeblock}

Note that the ID should be the same as the `file ID` used in the
`PUT` API. The name is not required, but for model training and
auto refresh the job result will be saved using the name defined
here. If name isn't defined, the {{site.data.keyword.pm_short}} service will
generate the result according to predefined naming rules.

### Job settings

All settings required to run this job.

```
"setting": {
     … Export settings
     … Import settings
     … Parameter value settings
     … Document control settings
}
```
{: codeblock}

### Database connectivity definitions

Database type: ApacheHive, DashDB, DB2, Greenplum, Impala, Informix, MySQL, Oracle, PostgreSQL, ProgressOpenEdge, Salesforce,  SQLServer, Sybase, SybaseIQ, Teradata.

Connectivity as specified in DB service instance, many DB types
require specific settings to be passed in ‘Options’

```
"dbDefinitions":{
     "db1":{
          "type":"DashDB",
          "host":"dashdb-enterprise-yp-dal09-11.services.dal.bluemix.net",
          "port":50001,
          "db":"BLUDB",
          "username":"bluadmin",
          "password":"NmI4MDViMzBkNDUz",
          "options":"Security=SSL"
     },
     "db2": {
          "type": "SQLServer",
          "db": "qatest",
          "host": "9.20.27.37",
          "port": 1433,
          "username": "sa",
          "password": "Pass1234",
          "options": "EnableQuotedIdentifiers=1"
     }
},
```
{: codeblock}

### Source node settings

Reference DB connectivity and table used to source a given branch
in the Modeler stream as identified by the source node name.

```
"inputs": [
     {
          "odbc": {
               "dbRef”; “db2”,
               "table": "DRUG1N",
          },
          "node": "ScoreInput"
     }
],
```
{: codeblock}

The table is the database table name. This table is used for
overriding the stream source node. The node is the data source
name for the stream. The dbRef references DB connectivity defined
by the dbDefinitions object, and table is the database table used
as the input of the given branch in the Modeler stream. The node
identifies the original source node in the Modeler stream to be
replaced by a DB source node constructed with these supplied
parameters.

### Export node settings

Reference DB connectivity and table used to persist data for a
given branch in the Modeler stream as identified by the terminal
node name.

Controlling method used when persisting data is communicated via
the insertMode attribute:

**Append** – table must exist and be compatible for insert

**Create** – table is created (an error is returned if it already
   exists)

**Drop** – if the referenced table exists, it is dropped and
   re-created

**Refresh** – existing rows from the table are deleted before
   inserting new rows
   
The table is the database table name to write the job results to.
The node is the terminal node name for the stream. Similar to the
Source node settings, node identifies the original Output node in
the Modeler stream to be replaced by a DB export node constructed
with these supplied parameters.

**Note**: The Export node settings and Source node settings are
important for making your job run successfully.

### Parameter value overrides

To override the default values for stream level parameters before
job execution, you can specify the name/value pairs to use in the
job definition.

```
"parameterOverride": [
     {
          "name": "p1",
          "value": "v1"
     },
     {
          "name": "p2",
          "value": "v2"
     }
],
```
{:codeblock}

Desired report formats for Evaluation document returned when
executing an Evaluation job type

All generated documents from the Evaluation job execution are
bundled into one .zip file. Only document builder nodes capable
of supporting the indicated reportFormat are executed and have
their content returned after successful execution of the job.

The following formats are supported: HTML, JPG, PNG, RTF, SAV, TAB, and XML.

```
"reportFormat": "HTML"
```
{: codeblock}

### Complete JSON example

```
{ 
     "action": "TRAINING", 
     "model": { 
          "id": "str2" 
          "name": "stream1.str" 
     },
     "dbDefinitions":{
          "db":{        
                    "type":"DashDB",
                    "host":"dashdb-enterprise-yp-dal09-11.services.dal.bluemix.net",        
                    "port":50001,        
                    "db":"BLUDB", 
                    "username":"bluadmin",  
                    "password":"NmI4MDViMzBkNDUz", 
                    "options":"Security=SSL"      
               }
     },
     "setting": {          
          "inputs": [
                    {
                         "odbc": {
                                   "dbRef”; “db”,
                                   "table": "DRUG1N",
                         },
                         "node": "ScoreInput"
                    }
          ],
          "parameterOverride": [
                    {
                        "name": "p1",
                        "value": "v1"
                    },
                    {
                        "name": "p2",
                        "value": "v2"
                    }
          ],
     }
}
```
{: codeblock}

## Batch job API details

The following sections provide batch job IBM® SPSS® Modeler file
management API details.

`PUT /v1/file/{id}`

Description: Uploads an IBM® SPSS® Modeler stream file for use in batch
jobs.

**Note**: If there is a file already stored under the specified ID,
it will be overwritten.

### Content Types

```
@Consumes({ "multipart/form-data"  })
@Produces({“application/json”})
```
{: codeblock}

### Parameters

Access key returned as credentials in provision or bind:

```
@QueryParam("accesskey") 
```
{: codeblock}

User-specified `file ID`. Must be unique in service instance
repository:

```
@PathParam("id") 
```
{: codeblock}

### Responses

Success. Returns the URL that can be used to download the file
from the service instance's repository:

```
@ApiResponse(code = 200)
```
{: codeblock}

Other error. JSON of exception returned:

```
@ApiResponse(code = 500)
```
{: codeblock}

`DELETE /v1/file/{id}`

Description: Deletes the file stored under the specified ID from
the service instance repository.

Content Types:

```
@Produces({“application/json”})
```
{: codeblock}

Parameters:

Access key returned as credentials in provision or bind:

```
@QueryParam("accesskey")
```
{: codeblock}

User-specified ID for the file when uploaded:

```
@PathParam("id") 
```
{: codeblock}

Responses:

Success. The specified file has been deleted:

```
@ApiResponse(code = 200)
```
{: codeblock}

Error. A file stored under this ID did not exist in the service
instance repository:

```
@ApiResponse(code = 404)
```
{: codeblock}

Other error. JSON of exception returned:

```
@ApiResponse(code = 500)
```
{: codeblock}

`GET /v1/file/`

Description: Returns a list of the IDs of all files being managed
in the service instance repository for batch job processing.

Content Types:

```
@Produces({“application/json”})
```
{: codeblock}

Parameters:

Access key returned as credentials in provision or bind:

```
@QueryParam("accesskey")  
```
{: codeblock}

Responses:

Success. Returns a JSON array of `file ID` values:

```
@ApiResponse(code = 200)
```
{: codeblock}

Other error. JSON of exception returned:

```
@ApiResponse(code = 500)
```
{: codeblock}

`GET /v1/file/{id}`

Description: Retrieves the IBM® SPSS® Modeler stream file stored for
use in the batch job processing under the specified ID.

Content Types:

```
@Produces({ "application/octet-stream" })
```
{: codeblock}

Parameters:

Access key returned as credentials in provision or bind:

```
@QueryParam("accesskey")  
```
{: codeblock}

User-specified ID for the file when uploaded:

```
@PathParam("id") 
```
{: codeblock}

Responses:

Success. Returns the IBM® SPSS® Modeler file:

```
@ApiResponse(code = 200)
```
{: codeblock}

Error. A file stored under this ID did not exist in the service
instance repository:

```
@ApiResponse(code = 404)
```
{: codeblock}

Other error. JSON of exception returned:

```
@ApiResponse(code = 500)
```
{: codeblock}

## Learn more

For an example of a batch job adoption in a notebook, see [From SPSS stream to batch scoring with
Python](https://apsportal.ibm.com/analytics/notebooks/9d7ce38e-9417-4c76-a6b9-5bc8cf40938a/view?access_token=5ca87e3007804e5b2bbbce77c20e99ac3c164d66f2d28dfffb54aa365caaef37).

For more information about the job definition JSON, see [Job
definition JSON](#job-definition-json).
