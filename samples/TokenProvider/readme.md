# Token Provider

Sample for how to use Azure App Service Authentication and Authorization (aka "Easy Auth") to provide rules based access to Resource Tokens in Cosmos DB. This is built upon Azure Functions as a easy way of hosting an HTTP endpoint, but you can use the concept in an ASP.NET or similar server based model.

The CosmosDBResourceTokenProvider project is the project with the logic around getting a Resource Token.

The TokenProvider project contains Azure Functions specific logic and translates our custom permission format to Cosmos DB permissions.

If you're using something other than JWT tokens to do auth, you'll need to change how TokenProvider makes its decision on which permission to fetch form the CosmosDBResourceTokenProvider, but you won't need to change the ResourceTokenProvider too much.

## Custom key format

The string format for the default role.

`path/to/resource[(partitionKey)][{permission}]`

partitionKey will be grabbed from the user claims on the jwt token. For instance, the user name. (usually `upn`)

For instance, to grant read access to a single partition key, you'd do:

`dbs/contoso/colls/cogs/(upn){READ}`

You can see more samples in the launchSettings.json


## Running locally

1. Install and start the Cosmos DB emulator.
2. Open solution in VS and make sure the Environment variables are set in the `TokenProvider` project
2. Build and start the `TokenProvider` project
3. Send a GET request to `http://localhost:7071/api/cosmos/token` with a `token` header containing a JWT token which has a `upn` claim
4. Response should contain an array of resource tokens 

## Roles

There is only 1 role supported today named "Default".

## Config

- `TOKEN_PROVIDER_COSMOS_ENDPOINT`: URL of the Cosmos DB resource (aka https://localhost:8081)
- `TOKEN_PROVIDER_COSMOS_MASTERKEY`: Master Key of the Cosmos DB resource
- `TOKEN_PROVIDER_COSMOS_DEFAULT`: Permission string for the "Default" role
- `TOKEN_PROVIDER_COSMOS_DEFAULT_KEYS`: Keys for environment variables that contain permission strings for the "Default" role
