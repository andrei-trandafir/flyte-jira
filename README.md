## Overview

![Build Status](https://travis-ci.org/HotelsDotCom/flyte-jira.svg?branch=master)
[![Docker Stars](https://img.shields.io/docker/stars/hotelsdotcom/flyte-jira.svg)](https://hub.docker.com/r/hotelsdotcom/flyte-jira)
[![Docker Pulls](https://img.shields.io/docker/pulls/hotelsdotcom/flyte-jira.svg)](https://hub.docker.com/r/hotelsdotcom/flyte-jira)

The Jira pack provides the ability to create issues, comment on issues and
to get info about issues.


## Build & Run
### Command Line
To build and run from the command line:
* Clone this repo
* Run `go build`
* Run `FLYTE_API_URL=http://.../ JIRA_HOST=https://... JIRA_USER=... JIRA_PASSWORD=... ./flyte-jira`
* Fill in this command with the relevant API url, jira host, jira user and jira password environment variables

### Docker
To build and run from docker
* Run `docker build -t flyte-jira .`
* Run `docker run -e FLYTE_API_URL=http://.../ -e JIRA_HOST=https://... -e JIRA_USER=... -e JIRA_PASSWORD=... flyte-jira`
* All of these environment variables need to be set

## Commands
This pack provides three commands: `CommentIssue`, `IssueInfo` and `CreateIssue`.
### issueInfo command
This command returns information about a specific issue.
#### Input
This commands input is the id of the desired issue:
```
"input" : "TEST-123",
```
#### Output
This command can either return an `Info` event or an `InfoFailure` event.
##### Info event
This is the success event, it contains the Id, Summary, Status, Description and Assignee. It returns them in the form:
```
"payload": {
    "id": "TEST-123",
    "summary": "Fix client race condition",
    "status": "In Progress",
    "description": "The client experiences.....",
    "assignee": "jsmith",
}
```
##### InfoFailure event
This contains the id of the issue and the error.
```
"payload": {
    "id" : "TEST-123",
    "error": "Could not get info on TEST-123: status code 400",
}
```

### CreateIssue command
This command creates a jira issue.
#### Input
This commands input is the project the issue should be created under, the issue type and the title.
```
"input": {
    "project": "TEST",
    "issue_type": "Story",
    "title": "Fix csetcd bug"
    }
```
#### Output
This command can return either a `CreateIssue` event or a `CreateIssueFailure` event.
##### CreatedIssue event
This is the success event, it contains the id of the issue and the url of the issue along with the input(project,
issue_type & title) It returns them in the form:
```
"payload": {
    "id": "TEST-123",
    "url": "https://localhost:8100/browse/TEST-123",
    "project": "TEST",
    "issue_type": "Story",
    "title": "Fix csetcd bug"
}
```
##### CreateIssueFailure event
This contains the error if the issue cannot be created along with the input (project, issue_type & title):
```
"payload": {
    "error": "Cannot create issue: Fix csetcd bug: status code 400",
    "project": "TEST",
    "issue_type": "Story",
    "title": "Fix csetcd bug"
}
```

### CommentIssue command
This command comments on an issue.
#### Input
This commands input is the id of the issue and the comment to be added.
```
"input": {
    "id": "TEST-123",
    "comment": "Added to backlog"
    }
```
#### Output
This command can return either a `Comment` event or a `CommentFailure` event. 
##### Comment event
This is the success event, it contains the id of the issue, the comment and the status.
Status is the status code returned when a comment is left successfully.
```
"payload": {
    "id": "TEST-123",
    "comment": "Added to backlog"
}
```
##### CommentFailure event
This returns the error if a issue cannot be commented on successfully. It contains, the issue id, the comment and
the error:
```
"payload": {
    "id": "TEST-123",
    "comment": "Added to backlog",
    "error": "Could not comment on issue: status code 400"
}
```
