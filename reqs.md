# GRIST
A highly opinionated Go RESTful API Stack backed by MongoDB

## Design Goals

* Provide easily extensible and overridable REST defaults
* Built in support to gather security credentials from cookies/session for browser based use, or access tokens and consumer keys for system -> system integration
* Built in support for api versioning, expressed in either URL numbering, or accepts header reading
* Framework built around the idea of extensible mux routers with sub-router support, and middleware stacks for end points.  

## Nice To Have Features

* JSON version of schemas exposed via ".schema" end points on each resource
* WebUI to investigate and actuate each resource end point

## Libraries and Approaches Used

* [Gorilla MUX](https://github.com/gorilla/mux) for the router & sub-routers
* [Negroni](https://github.com/codegangsta/negroni) used for middleware stack
* [mgo](https://labix.org/mgo) used for connecting to Mongo
* Develop and use "Gomodamo" as a go mongo data modeling framework
*  provide a "data model" approach for data manipulation functions (save, update, delete, etc)
*  provide a series of pluggable middleware functions to enable common query needs like filtering listed data, pagination, specifying sub-objects to include in the results
*  code geneation via commandline tool that will be used to read resource model files and generate the function extensions needed to implement modeling, plugable functionaility, and rest defaults

## Target Functionality

* provide pluggable functionality using struct tag based reflection
* will provide population and sudo-join capabilities via the resource model properties
* will provide a standard set of query string operations for filtering, joining, population, pagination, and sorting


## struct tags

Go struct tags will be used to instruct the grist commandline tool to include plug-in functionality at the mongo data model level.  The tag used will be 'grist', and will include sub properties that are comma seperated, and can have a equals sign to assign a value.  The supported properties are: 

|property|required type|details|
|---|-------------|-------|
|fauxdelete|Boolean|indicates that grist should enable faux deletion for the resource and that the 'existence' of this record is dependent on a true value in the field this property is found on. |
|createdat|time.Time|field marked with this attribute should be auto-populated with a timestamp when created time if no other value is present|
|updatedat|time.Time|field marked with this attribute should be auto-populated with a timestamp every time an update is saved to the database|
|deletedat|time.Time|field marked with this property should be auto-populated with a timestamp when the record is faux-deleted.  *depends on a fauxdelete filed to be defined as well, has no effect if no fauxdelete field is supplied|

### example of tag usage: 

