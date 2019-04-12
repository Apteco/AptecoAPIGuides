# Apteco API - Generating an API Client Library
The Apteco API has been built with the the [Swagger/Open API Specification](https://swagger.io/resources/open-api/).
Tools are available to automatically generate client-side libraries for all the API endpoints to speed up integration
and handle common tasks such as authentication.

## Contents

* [Generating client-side libraries with swagger-codegen](#generating-client-side-libraries-with-swagger-codegen)
* [Client Library Usage](#client-library-usage)

## Generating client-side libraries with swagger-codegen

[swagger-codegen](https://github.com/swagger-api/swagger-codegen) is a tool that will connect to the Apteco API specification
and generate a client-side library in a number of different languages.

An example of a generated library in C# (including notes on how it was generated) can be found in the
[ApiClient](https://github.com/Apteco/ApiSystemAnalyser/tree/master/Apteco.ApiSystemAnalyser.ApiClient) part of the
[ApiSystemAnalyser](https://github.com/Apteco/ApiSystemAnalyser) sample application

The steps to go through are:

1. Ensure you have Java installed (at the time of writing swagger-codegen uses Java 8).
2. Download the latest stable release of the swagger-codegen (see 
[the prerequisites on the swagger-codegen readme](https://github.com/swagger-api/swagger-codegen#prerequisites)
for a link to get the jar file from).
3. Create a json options file to contain the settings to use when generating the API client code.  This will be languge
dependant - see the swagger-codegen documentation for details.  There is an example C# options file in the
[ApiSystemAnalyser](https://github.com/Apteco/ApiSystemAnalyser) sample application.
4. Run the following command to generate the client libraries from the API hosted at the given URL:

```
java -jar swagger-codegen-cli.jar generate -i http://www.tealgreenholidays.co.uk/OrbitAPI/swagger/v2/swagger.json
-l csharp -o GeneratedClient -c .\swagger-codegen-options.json
```

If you want to generate the client against your own API install then simply change the URL.

## Client library usage
To call the API via the client library you could use code similar to below (C# example):

``` csharp
private async Task LoginAndListAvailableFastStatsSystems()
{
  string dataViewName = "holidays";
  string userLogin = "user";
  string password = "letmein";

  var sessionsApi = new SessionsApi(CreateConfiguration(null));
  var sessionDetails = await sessionsApi.SessionsCreateSessionSimpleAsync(dataViewName, userLogin, password);

  var fastStatsSystemsApi = new FastStatsSystemsApi(CreateConfiguration(sessionDetails));
  var pagedSystems = await fastStatsSystemsApi.FastStatsSystemsGetFastStatsSystemsAsync(dataViewName);
  foreach (var system in pagedSystems.List)
  {
    Console.WriteLine(system.Name);
  }

  await sessionsApi.SessionsLogoutSessionAsync(dataViewName, sessionDetails.SessionId);
}

private Configuration CreateConfiguration(SessionDetails sessionDetails)
{
  var defaultHeaders = new Dictionary<string, string>();
  if (sessionDetails != null)
  {
    defaultHeaders.Add("Authorization", "Bearer " + sessionDetails.AccessToken);
  }

  return new Configuration()
  {
    DefaultHeader = defaultHeaders,
    BasePath = baseUrl
  };
}
```

The above example logs in with a given username and password and then outputs each of the available FastStats
system names to the console.

Each part of the API has its own class (i.e. `SessionsAPI`, `FastStatsSystemsApi`, etc).  See the API swagger documentation
(such as at http://www.tealgreenholidays.co.uk/OrbitAPI/swagger/ui/index.html) for details on what each call does.