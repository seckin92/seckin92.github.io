---
layout: post
title: Perform CRUD Operations using `Xrm.WebApi`
---

{{ page.title }}
================

<p class="meta">18 September 2019, Birmingham, UK</p>

I have recently had a challenge where I needed to perform some CRUD operations on a bunch of records, and wondered whether I could use the Client API `Xrm.WebApi.online.executeMultiple` to achieve this.

While there are helper methods to perform CRUD operations on a single record, there were no examples of this in the PowerApps documentation, so I decided to add my own in [PR#626](https://github.com/MicrosoftDocs/powerapps-docs/pull/626) (any comments/improvements appreciated!).

Here are the examples I've come up with, hope this saves someone some time!:

#### Create a record

The following example demonstrates how to perform a Create operation.

```JavaScript
var Sdk = window.Sdk || {};
/**
 * Request to execute a create operation
 */
Sdk.CreateRequest = function (entityName, payload) {
    this.etn = entityName;
    this.payload = payload;
    this.getMetadata = function () {
        return {
            boundParameter: null,
            parameterTypes: {},
            operationType: 2, // This is a CRUD operation. Use '0' for actions and '1' for functions
            operationName: "Create",
        };
    };
};
// Construct a request object from the metadata
var payload = {};
payload["name"] = "Fabrikam Inc.";
var createRequest = new Sdk.CreateRequest("account", payload);
// Use the request object to execute the function
Xrm.WebApi.online.execute(createRequest).then(
    function (result) {
        if (result.ok) {
            console.log("Status: %s %s", result.status, result.statusText);
            // perform other operations as required;
        }
    },
    function (error) {
        console.log(error.message);
        // handle error conditions
    }
);
 ```

#### Retrieve a record

The following example demonstrates how to perform a Retrieve operation.

```JavaScript
var Sdk = window.Sdk || {};
/**
 * Request to execute a retrieve operation
 */
Sdk.RetrieveRequest = function (entityReference, columns) {
    this.entityReference = entityReference;
    this.columns = columns;
    this.getMetadata = function () {
        return {
            boundParameter: null,
            parameterTypes: {},
            operationType: 2, // This is a CRUD operation. Use '0' for actions and '1' for functions
            operationName: "Retrieve",
        };
    };
};
// Construct a request object from the metadata
var retrieveRequest = new Sdk.RetrieveRequest({ etn: "account", id: "87547d08-b9d0-e911-a826-000d3a43d70a" }, ["name"]);
// Use the request object to execute the function
Xrm.WebApi.online.execute(retrieveRequest).then(
    function (result) {
        if (result.ok) {
            console.log("Status: %s %s", result.status, result.statusText);
            // perform other operations as required;
        }
    },
    function (error) {
        console.log(error.message);
        // handle error conditions
    }
);
```

#### Update a record

The following example demonstrates how to perform a Update operation.

```JavaScript
var Sdk = window.Sdk || {};
/**
 * Request to execute an update operation
 */
Sdk.UpdateRequest = function (entityName, entityId, payload) {
    this.etn = entityName;
    this.id = entityId;
    this.payload = payload;
    this.getMetadata = function () {
        return {
            boundParameter: null,
            parameterTypes: {},
            operationType: 2, // This is a CRUD operation. Use '0' for actions and '1' for functions
            operationName: "Update",
        };
    };
};
// Construct a request object from the metadata
var payload = {};
payload["name"] = "AdventureWorks Inc.";
var updateRequest = new Sdk.UpdateRequest("account", "87547d08-b9d0-e911-a826-000d3a43d70a", payload);
// Use the request object to execute the function
Xrm.WebApi.online.execute(updateRequest).then(
    function (result) {
        if (result.ok) {
            console.log("Status: %s %s", result.status, result.statusText);
            // perform other operations as required;
        }
    },
    function (error) {
        console.log(error.message);
        // handle error conditions
    }
);
```

#### Delete a record

The following example demonstrates how to perform a Delete operation.

```JavaScript
var Sdk = window.Sdk || {};
/**
 * Request to execute a delete operation
 */
Sdk.DeleteRequest = function (entityReference) {
    this.entityReference = entityReference;
    this.getMetadata = function () {
        return {
            boundParameter: null,
            parameterTypes: {},
            operationType: 2, // This is a CRUD operation. Use '0' for actions and '1' for functions
            operationName: "Delete",
        };
    };
};
// Construct a request object from the metadata
var deleteRequest = new Sdk.DeleteRequest({ entityType: "account", id: "87547d08-b9d0-e911-a826-000d3a43d70a" });
// Use the request object to execute the function
Xrm.WebApi.online.execute(deleteRequest).then(
    function (result) {
        if (result.ok) {
            console.log("Status: %s %s", result.status, result.statusText);
            // perform other operations as required;
        }
    },
    function (error) {
        console.log(error.message);
        // handle error conditions
    }
);
```