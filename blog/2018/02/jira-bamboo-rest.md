+++
date = "2018-02-13"
title = "Useful Jira and Bamboo REST Endpoints"
math = "true"
categories = "test"
+++

In this Blog Post, I list some of the most useful REST API Endpoints in Bamboo and Jira.

### Bamboo

##### Execute a Build Plan

Initiate an execution of a Build Plan using the REST API. You can specify the Project Key and Build Plan Key in the URL and this will start a manual execution of the Plan.

~~~~
{
    method: "POST",
    path: "/queue/{Project-Key}-{Build-Key}"
    }
    ~~~~

You can execute all Stages with the following argument:

`?executeAllStages=true`

Note that you can specify custom variables using the following syntax at the end of the URL:

`?bamboo.variable.myVariable=valueForMyVariable`

##### Get all Queued Jobs

Returns a list of all Queued Jobs currently waiting, or in progress.

~~~~
{
    method: "GET",
    path: "/queue"
    }
    ~~~~

##### Get Queued Jobs of a certain Build Plan

Get a list of any Jobs currently being run of a certain Build Plan.

~~~~
{
    method: "GET",
    path: "/queue/{Project-Key}-{Build-Key}"
    }
    ~~~~

##### Get Version Status of Deployment

Get the current deployment state of a release version.

~~~~
{
    method: "GET",
    path: "/deploy/version/{Version-ID}/status"
    }
    ~~~~

##### Clone a Plan

Use a Post Request to clone a Build Plan into another Build Project.

~~~~
{
    method: "POST",
    path: "/clone/{projectKey}-{buildKey}:{toProjectKey}-{toBuildKey}"
    }
    ~~~~

##### Get System Information

Get the Bamboo system status, including it’s version and build information and running status.

This is useful for monitoring solutions to determine the status of Bamboo Server.

~~~~
{
    method: "GET",
    path: "/info"
    }
    ~~~~

### Jira

##### Get an Issue

Get the issue details, including the summary, priority, description and custom field values.

~~~~
{
    method: "GET",
    path: "/issue/<issueKey>"
        }
        ~~~~

##### Search for Issues

This endpoint lets you perform a JQL Search on Jira. This is very customisable and can be used to pull back issues which match your criteria. Some examples are below:

* "project=TS AND fixVersion=10000" – Issues in a certain Version and Project
* "status=Done AND project=ST" – Done issues in the ST Project

~~~~
{
    method: "GET",
    path: "/search?jql={jqlQuery}"
    }
    ~~~~

##### Transition Issue

This lets you transition an Issue to a certain status provided that the transition is valid.

~~~~
{
    method: "POST",
    path: "/issue/<issueKey>}/transitions?expand=transitions.fields",
        requestBody: {
            "transition": {
                "id": <transitionId>
                    }
                    }
                    }
                    ~~~~

##### Edit Issue

Edit an issue, in this case changing the assignee of an issue.

~~~~
{
    method: "PUT",
    path: "/issue/<issueKey>}",
        requestBody: {
            "fields": {
                "assignee": {
                    "name": "<newAssignee>"
                        }
                        }
                        }
                        }
                        ~~~~

##### Get Version Information

Get information on a version, including its name, start date, release date and release status.

~~~~
{
    method: "GET",
    path: "/version/<versionId>",
        }
        ~~~~

##### Get Project Information

Get Project information, including Version information, Issue Types and Project Lead.

~~~~
{
    method: "GET",
    path: "/project/{projectId}",
    }
    ~~~~

## Note on Libraries

We don’t want to use REST API’s if we are using a language that has a Jira or Bamboo library, such as Python or Java:

* [Jira Library for Python](https://jira.readthedocs.io/en/master/)
* [Jira Library for Java](https://github.com/rcarz/jira-client)

These libraries allow us to write more consistent code, for example, in Python:

##### Get an Issue

~~~~
issue = jira.issue('JE-12')`
~~~~

##### Assign an Issue

~~~~
jira.assign_issue(issue, 'newassignee')
~~~~

##### Create a new Issue

~~~~
issue_dict = {
    'project': {'id': 123},
    'summary': 'New issue',
    'description': 'Look at me',
    'issuetype': {'name': 'Bug'},
    new_issue = jira.create_issue(fields=issue_dict)
    ~~~~

As you can see, a library can make integration work easier to implement and keep up to date by using repositories.