# Okta Quarkus Sample

This example shows you how to use the [Quarkus's OpenID Connect Adapter][] to login a user. The login is achieved through the [Authorization Code Flow][] where the user is redirected to the Okta-Hosted login page. After the user authenticates, they are redirected back to the application and a local cookie session is created.

It also shows how to return the user's information from an API using an OAuth 2.0 access token.

## Prerequisites

Before running this sample, you will need the following:

* [Java 17+](https://sdkman.io/jdks)
* An Okta Integrator Free Plan account. To get one, sign up for an [Integrator account](https://developer.okta.com/login). Once you have an account, sign in to your [Integrator account](https://developer.okta.com/login). Next, in the Admin Console:

1. Go to **Applications > Applications**
2. Click **Create App Integration**
3. Select **OIDC - OpenID Connect** as the sign-in method
4. Select **Web Application** as the application type, then click **Next**
5. Enter an app integration name
6. Configure the redirect URIs:
- Accept the default redirect URI values:
- **Sign-in redirect URIs:** `http://localhost:8080/callback`
- **Sign-out redirect URIs:** `http://localhost:8080`
7. In the **Controlled access** section, select the appropriate access level
8. Click **Save**

Creating an OIDC Web App manually in the Admin Console configures your Okta Org with the application settings. You may also need to configure trusted origins for `http://localhost:8080` in **Security > API > Trusted Origins**.

## Get the Code

```bash
git clone https://github.com/okta-samples/okta-quarkus-sample.git
cd okta-quarkus-sample
```

Update your `.env` file with the values from your application's configuration:

```text
ISSUER=https://dev-133337.okta.com
CLIENT_ID=0oab8eb55Kb9jdMIr5d6
CLIENT_SECRET=NEVER-SHOW-SECRETS
```

### Where are my new app's credentials?

After creating the app, you can find the configuration details on the app’s **General** tab:
- **Client ID:** Found in the **Client Credentials** section
- **Client Secret:** Click **Show** in the **Client Credentials** section to reveal
- **Issuer:** Found in the **Issuer URI** field for the authorization server that appears by selecting **Security > API** from the navigation pane.

## Enable Refresh Token

Sign into your [Okta Developer Edition account](https://developer.okta.com/login/) to add a required setting to your Quarkus Okta app to avoid third-party cookies. Navigate to **Applications** > **Applications** and select "okta-quarkus-sample" application to edit. Find the **General Settings** and press **Edit**. Enable **Refresh Token** in the **Grant type** section. **Save** your changes.

## Run the Example

```bash
./mvnw quarkus:dev
```

Log in at `http://localhost:8080`.

## API Access with OAuth 2.0

You can also retrieve user information from the `/hello` endpoint with an OAuth 2.0 access token.

First, you'll need to generate an access token.

1. Run `okta apps create spa`. Set `oidcdebugger` as an app name and press **Enter**.

2. Use `https://oidcdebugger.com/debug` for the Redirect URI and set the Logout Redirect URI to `https://oidcdebugger.com`.

3. Navigate to the [OpenID Connect Debugger website](https://oidcdebugger.com/).

    1. Fill in your client ID
    2. Use `https://{yourOktaDomain}/oauth2/default/v1/authorize` for the Authorize URI
    3. Select **code** for the response type and **Use PKCE**
    4. Click **Send Request** to continue

4. Set the access token as a `TOKEN` environment variable in a terminal window.

       TOKEN=eyJraWQiOiJYa2pXdjMzTDRBYU1ZSzNGM...

5. Test the API with [HTTPie](https://httpie.io/cli) and an access token.

       http :8080/hello Authorization:"Bearer $TOKEN"

## Learn More

For more details on how to build an application with Okta and Quarkus you can read [Secure Kafka Streams with Quarkus and Java](https://developer.okta.com/blog/2020/04/08/kafka-streams).

[Quarkus's OpenID Connect Adapter]: https://quarkus.io/guides/security-openid-connect
[OIDC Web Application Setup Instructions]: https://developer.okta.com/docs/guides/implement-grant-type/authcode/main/#set-up-your-app
[Authorization Code Flow]: https://developer.okta.com/docs/guides/implement-grant-type/authcode/main/
