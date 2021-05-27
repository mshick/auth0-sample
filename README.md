# FOR TAKESHAPE PR TESTING

1. Create an Auth0 account.
2. Create an Auth0 application (Applications > Applications), be sure to provide `http://localhost:3010` to Allowed Callback URLs, Allowed Logout URLs and Allowed Web Origins
3. Create an Auth0 service in your TakeShape project. Use your Auth0 domain and provide an ID. Copy the `audience` from the TakeShape config screen. Save the service.
4. Create an Auth0 API (Applications > APIs). Use the audience you copied from the TakeShape UI.
5. Add a scope to the Auth0 API config to grant a takeshape role. Use a builtin, so add, `takeshapeRole:read`.
6. Clone this repo. Add a file `src/auth_config.json`. Fill it with the following, with your own Auth0 instance info.

```json
{
  "domain": "YOUR_AUTH0_DOMAIN",
  "clientId": "YOUR_AUTH0_CLIENT_ID",
  "audience": "YOUR_AUDIENCE_FROM_TAKESHAPE",
  "apiOrigin": "YOUR_API_ORIGIN",
  "scope": "takeshapeRole:read"
}
```

> For the PR, `YOUR_API_ORIGIN` will be along the lines of `https://pr2850.api.dev.takeshape.io/project/35357479-4135-4a9a-b2ec-72bb0c7199db/v3/graphql`.

7. `npm install` the project
8. `npm run spa` to start the client
9. Create a multiple shape in TakeShape called `Profile` with fields `id` (single line string), `fullName` (single line string) and `bio` (paragraph). 
10. Add the following query by editing your schema

```json
    "getMyProfile": {
      "shape": "Profile",
      "resolver": {
        "name": "takeshape:find",
        "service": "takeshape:local",
        "options": {"model": "Profile"},
        "argsMapping": {"where.id.eq": [["get", {"path": "claims.sub"}]]}
      },
      "description": "Get a Profile by ID",
      "args": {"type": "object", "properties": {}}
    },
 ```
 
 11. Sign up for an Auth0 account using the locally running SPA. Once created, click on the `Profile` link from the dropdown and copy your `sub`. It will be similar to `auth0|60945b2cb1cc580071d093dc`.
 12. Add the sub id as the `id` in a new Profile. Fill in the other values.
 13. Click on `External API` in the SPA. Click the `Ping API` button. You should see a valid query result...
 14. 

# Auth0 React SDK Sample Application

This sample demonstrates the integration of [Auth0 React SDK](https://github.com/auth0/auth0-react) into a React application created using [create-react-app](https://reactjs.org/docs/create-a-new-react-app.html). The sample is a companion to the [Auth0 React SDK Quickstart](https://auth0.com/docs/quickstart/spa/react).

This sample demonstrates the following use cases:

- [Login](https://github.com/auth0-samples/auth0-react-samples/blob/master/Sample-01/src/components/NavBar.js#L72-L79)
- [Logout](https://github.com/auth0-samples/auth0-react-samples/blob/master/Sample-01/src/components/NavBar.js#L102-L108)
- [Showing the user profile](https://github.com/auth0-samples/auth0-react-samples/blob/master/Sample-01/src/views/Profile.js)
- [Protecting routes](https://github.com/auth0-samples/auth0-react-samples/blob/master/Sample-01/src/views/Profile.js#L33)
- [Calling APIs](https://github.com/auth0-samples/auth0-react-samples/blob/master/Sample-01/src/views/ExternalApi.js)

## Project setup

Use `npm` to install the project dependencies:

```bash
npm install
```

## Configuration

### Create an API

For the ["call an API"](https://auth0.com/docs/quickstart/spa/react/02-calling-an-api) page to work, you will need to [create an API](https://auth0.com/docs/apis) using the [management dashboard](https://manage.auth0.com/#/apis). This will give you an API identifier that you can use in the `audience` configuration field below.

If you do not wish to use an API or observe the API call working, you should not specify the `audience` value in the next step. Otherwise, you will receive a "Service not found" error when trying to authenticate.

### Configure credentials

The project needs to be configured with your Auth0 domain and client ID in order for the authentication flow to work.

To do this, first copy `src/auth_config.json.example` into a new file in the same folder called `src/auth_config.json`, and replace the values with your own Auth0 application credentials, and optionally the base URLs of your application and API:

```json
{
  "domain": "{YOUR AUTH0 DOMAIN}",
  "clientId": "{YOUR AUTH0 CLIENT ID}",
  "audience": "{YOUR AUTH0 API_IDENTIFIER}",
  "appOrigin": "{OPTIONAL: THE BASE URL OF YOUR APPLICATION (default: http://localhost:3000)}",
  "apiOrigin": "{OPTIONAL: THE BASE URL OF YOUR API (default: http://localhost:3001)}"
}
```

**Note**: Do not specify a value for `audience` here if you do not wish to use the API part of the sample.

## Run the sample

### Compile and hot-reload for development

This compiles and serves the React app and starts the backend API server on port 3001.

```bash
npm run dev
```

## Deployment

### Compiles and minifies for production

```bash
npm run build
```

### Docker build

To build and run the Docker image, run `exec.sh`, or `exec.ps1` on Windows.

### Run your tests

```bash
npm run test
```

## Frequently Asked Questions

If you're having issues running the sample applications, including issues such as users not being authenticated on page refresh, please [check the auth0-react FAQ](https://github.com/auth0/auth0-react/blob/master/FAQ.md).

## What is Auth0?

Auth0 helps you to:

* Add authentication with [multiple sources](https://auth0.com/docs/identityproviders), either social identity providers such as **Google, Facebook, Microsoft Account, LinkedIn, GitHub, Twitter, Box, Salesforce** (amongst others), or enterprise identity systems like **Windows Azure AD, Google Apps, Active Directory, ADFS, or any SAML Identity Provider**.
* Add authentication through more traditional **[username/password databases](https://auth0.com/docs/connections/database/custom-db)**.
* Add support for **[linking different user accounts](https://auth0.com/docs/users/user-account-linking)** with the same user.
* Support for generating signed [JSON Web Tokens](https://auth0.com/docs/tokens/json-web-tokens) to call your APIs and **flow the user identity** securely.
* Analytics of how, when, and where users are logging in.
* Pull data from other sources and add it to the user profile through [JavaScript rules](https://auth0.com/docs/rules).

## Create a Free Auth0 Account

1. Go to [Auth0](https://auth0.com) and click **Sign Up**.
2. Use Google, GitHub, or Microsoft Account to login.

## Issue Reporting

If you have found a bug or if you have a feature request, please report them at this repository issues section. Please do not report security vulnerabilities on the public GitHub issue tracker. The [Responsible Disclosure Program](https://auth0.com/responsible-disclosure-policy) details the procedure for disclosing security issues.

## Author

[Auth0](https://auth0.com)

## License

This project is licensed under the MIT license. See the [LICENSE](../LICENSE) file for more info.
