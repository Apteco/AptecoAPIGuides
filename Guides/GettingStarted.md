# Apteco API - Getting Started
The Apteco API is a RESTful API that provides access to functionality within the [Apteco Marketing Suite™](http://www.apteco.com).
It is designed to allow you to create applications and integrations with data available from within the Apteco Marketing Suite™.

This guide will help you through the process of developing such applications.

## Contents

* [Setting up and configuring the Apteco API](#setting-up-and-configuring-the-apteco-api)
* [A quick tour](#a-quick-tour)
  * [Swagger UI](#swagger-ui)
  * [Authentication](#authentication)

## Setting up and configuring the Apteco API
The Apteco API is available as part of the Apteco Marketing Suite™.  For details on installing the Apteco API, please contact
your Apteco partner or Apteco Support.

The Apteco API is typically installed as part of the Apteco Orbit™ platform and the Apteco Orbit™ browser-based UI depends on
the Apteco API to function.  If you already have Apteco Orbit™ installed then the Apteco API will be available.

## A quick tour

### Swagger UI
Once you have the Apteco API installed you should be able to browse to the online documentation:

![SwaggerUI Index Page](Images/GettingStarted/SwaggerUI-Index.png)

The URL of this page will depend on your installation, but if the API was installed at
https://www.example.com then the API would typically be available from https://www.example.com/OrbitAPI/swagger/ui

The API has been developed with the [Swagger/Open API Specification](https://swagger.io/resources/open-api/).
The Swagger User Interface shown above allows you to browse through the endpoints that are available in the API.

For each endpoint, you can then delve into the parameters and expected inputs and outputs.  The Swagger UI also 
allows you to enter values and try out the endpoint to see what it returns.  This is very useful when
developing applications against the API:

![SwaggerUI FastStatsSystems/{systemName} endpoint](Images/GettingStarted/SwaggerUI-FastStatsSystems-SystemName.png)

### Authentication

Some endpoints in the Apteco API (such as the /About/Version endpoint) don't require authentication before they
can be accessed but most functionality is dependent on having valid user credentials.  The sample applications
([mentioned here](../Readme.md#sample-applications) show how to programatically perform authentication, but to
access endpoints that require authentication using the Swagger UI:

1. Go to the Sessions/SimpleLogin endpoint:

![SwaggerUI Sessions/SimpleLogin Request](Images/GettingStarted/SwaggerUI-Sessions-SimpleLogin-Request.png)

2. Enter the Data View you want to log in to, the "UserLogin" (either the FastStats username or email address
of the user you want to log in as) and the user's password.  Press the "Execute" button to call the endpoint.

3. The result of the call will be something like the following, with the first part of the returned JSON
4. containing an accessToken.  This is the [JWT](https://jwt.io/) that is used to authenticate with the API.

![SwaggerUI Sessions/SimpleLogin Results](Images/GettingStarted/SwaggerUI-Sessions-SimpleLogin-Results.png)

4. Copy the contents of the accessToken into your clipboard and then go to the top of the Swagger UI page
and click on the "Authorize" button.  Enter the string "Bearer" then a space and then the accessToken.
Then press Authorize to log in.

![SwaggerUI Authorize Dialog](Images/GettingStarted/SwaggerUI-AuthorizeDialog.png)

5. You will now be able to access all of the endpoints that require an authenticated user.