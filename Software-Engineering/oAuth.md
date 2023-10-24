- for authorization
- giving third and first party applications "clients" access to your application without having them to know the users password of your application
- the first / third party app provides redirects the user to a login page of the "authorization" server where the users provide their credentials and get redirected back to the first / third party application ("client") which receives an access token (and refresh token) to make calls in name of the user to your application
- whole login flow is managed by authorization server and changes to the authorization flow does not effect other first / third party apps, because the result of an access token remains the same
- with client id of the client you are able to track and audit from where (which App / API) the calls are coming


# oAuth vs OIDC
- oAuth for authorization to access an API (without having to know who the user is)
	- e.g.: in a hotel where you check in and show you ID to the personal staff which provide you a key ("access token")
	- the key can be used to open the door without knowing who you are (only the staff team know you authorized you with a key to the room)
	- so a third party app does not have to know who a user is to upload a document to a specific google drive. it just needs access to it and has to know which drive ("hotel") to use
- OIDC (Open ID Connect) is an extension to oauth (so also implements the authorization) but just adds and identification layer to the process where the first / third party app gets user information via an "id token".
- with the extension of OIDC oauth becomes more like a single sign on protocol
- an id_token can be obtained over multiple ways, where some of the require your to validate the signature of the id_token
	- providing `oidc` as scope to get the id_token along with the access_token (id_token must not be validated) This is the most secure way to get an id_token. in bets way with PKCE
	- in all other ways you should validate the id_token e.g. when using id_token as grant_type (because this is just using an implicit flow with is not secure) (validate the signature of the jwt AND validating the issuer AND validating the audience AND nonce value AND)


# oAuth Roles

## Resource Owner
- person / end user who is giving access to some portion (scopes) of their account
## Client
- third / first party application that is attempting to get access to the user's account
- needs permission from the user before it can do 

### Client Types
- public client: cannot include client secret because it runs in the browser or in a mobile app, where the code is exposed
- confidential client: can include client secret, because it runs on a secured server

## Resource Server
- API server used to access the user's information (your app)

## Authorization Server
- server that presents the interface where the user approves or denies the request
- In smaller implementations, this may be the same server as the API server, but larger scale deployments will often build this as a separate component
- providing login interface
- providing (optional) consent screen where the user confirms that the first / third party app will be granted access for specific scopes

# Components

## Scope
- scopes what the client can do on behalf of the resource owner (like reading profile data, writing profile data, writing emails and so on...)

## Redirect URIs
- service will only redirect users to a registered URI, which helps prevent some attacks
- location of the client where authorization server is going to send the user back to after they log in
- redirect URIs are registered at the authorization server, so the authorization server is sending back only to the registered redirect UI
- this is the only chance to identify that a public client is really the client, because a domain is unique and only manageable from the DNS owner. It is not top secure, but it is the best what we have until know because the client secret is not an option in public clients
- Any HTTP redirect URIs must be served via HTTPS

## Client Id and Secret
- After registering your app, you will receive a client ID and optionally a client secret to identify your client
- client ID is considered public information, and is used to build login URLs, or included in Javascript source code on a page
- client secret **must** be kept confidential. If a deployed app cannot keep the secret confidential, such as single-page Javascript apps or native apps, then the secret is not used, and ideally the service shouldn't issue a secret to these types of apps in the first place
- the client secret is used to identify that the request is really from a specific client / app. This is important because you might handle thing different for another client (e.g including roles in first party app and not in third party app or rate limits API access or access to specific resources or showing a consent screen only for third party apps or token life times, providing refresh token etc...)


# Front vs Back Channel
## Front Channel
- using a browsers address bar to move data between two other pieces of software
- less secure
- redirects of authorization flow are via front channel
- for that we create in a lot of grant types an "authorization code" which is short lived in exchange over the front channel (redirect) and get the access / refresh token based on the authorization token over the backend channel (http)

## Back Channel
- HTTP client (or server) that makes a request to an HTTP server
- even if that client is JavaScript code in a browser
- more secure
- for that we create in a lot of grant types an "authorization code" which is short lived in exchange over the front channel (redirect) and get the access / refresh token based on the authorization token over the backend channel (http)

# PKCE
- Proof Key for Code Exchange
- pronounced “pixie”
- extension to the authorization code flow to prevent CSRF and authorization code injection attacks
- client first creating a secret on each authorization request, and then using that secret again when exchanging the authorization code for an access token
- at first for SPA and mobile apps to replace the implicit grant flow. Today it should also be used for all authorization code flows. also for confidential clients


# Grant Types

# Authorization Code
- authorization code is a temporary code that the client will exchange for an access token
- authorization code itself is obtained from the authorization server where the user gets a chance to see what the information the client is requesting, and approve or deny the request
- mostly used with PKCE
- mostly used for server app or SPA / mobile apps 
1. user wants to use another app
2. client generates a new secret and hashes it (for PKCE)
3. client generates a request for authorization from the user via "front channel". This is accomplished by creating an authorization request link for the user to click on like:
   ```
   https://authorization-server.com/oauth/authorize
	?client_id=CLIENT_ID
	&response_type=code
	&state=5ca75bd30 # random string for csrf protection
	&redirect_uri=https%3A%2F%2Fexample-app.com%2Fauth
	&scope=photos
	&code_challenge=XXXXXXX # hashed PKCE secret
	&code_challenge_method=S256
   ```
4. user logs in at the authorization server and accepts the consent screen
5. authorization server issues an temporary authorization code to the user
6. the user provides the authorization code to the client
7. the client requests an access token from the authorization server providing the authorization code and the previous generated secret as plaintext via the back channel like:
   ```
   POST https://authorization-server.com/oauth/token
	?client_id=CLIENT_ID
	&client_secret=CLIENT_SECRET
	&grant_type=authorization_code
	&code=AUTHORIZATION_CODE
	&redirect_uri=https%3A%2F%2Fexample-app.com%2Fauth
	&code_verifier=XXXXXXX
   ```
8. the server validates the secret and authorization code and returns a response like:
   ```json
   {
	   "token_type": "Bearer",
	   "access_token": "XXXXXX",
	   "expires_in": 3600,
	   "scope": "photos",
	   "refresh_token": "XXXX"
   }
   ```

### Authorization Code Flow for Native Mobile Apps
Problems: 
- mobile URLs like `example-app://` are unsecure because everyone can claim it so it get redirected to a different App with the authorization code. but this can also be handled with a real Domain, where only one App has access to and which can be configures by the DNS Admin
- WebViews are also unsecure for the authorization screen because:
	- user does not see the url in the web view (so it could be a malicious site)
	- the app can access data of the webview (and the whole point of oauth is that you wanna prevent that)
	- Webview cant access global Cookies of the device
	- -> this all could be solved via a operation system webbrowser which coould be open by an app. Here we see the whole URL, cann access the global Cookies and the App cannot access the data of it. In IOS its called `SFSafariViewController`and in Android `Chrome Custom Tabs`
- still storing the client secret inside the mobile app is not a secure option, because the code can be decompiled and so on... So for that we should use PKCE without a client secret, which can often results in restrictions to the refresh token (see example for Single Page Applications below)

### Authorization Code Flow for Single Page Applications
Problems: 
- in comparison to mobile apps its easier realted to opening a secure browser because thats the case by default in a browser. so no issues with a web view (see example for mobile apps above)
- but the storage of the access token (and refresh token) and client secret is a big problem. because there is no secure way with support for all browsers. XSS attacks can be really dangerous
- because of that you also need to use PKCE without a client secret. A lot of authorization servers dont issue a refresh token with that or restrict it very hard (e.g. Java Spring Authorization Server which does not provide an refresh token for public clients) This can be big problem because you dont wanna let users re log in very time and also dont wanna set the live time of an access token too high.

Solution:
- because of that you should not store the access and refresh tokens in the browser. You should have a BFF (Backend for frontend) which acts as confidential client and holds the access and refresh token. The BFF creates a session for the SPA which should be stored in a HttpOnly Cookie. This session is used for authentication the frontend to the backend. The BFF then replaces the Session auth with an Access Token via a `tokenDelay`The BFF also refreshes the access token. The SPA might also be able to request the access token from the BFF for direct access to the Resource Server, but NOT the refresh token. And the Access token should then not be stored in the frontend!
Flow:
1. user wants to sign in
2. SPA redirects user to the BFF
3. the BFF redirects the user to the authorizations login page (so the BFF creates the frontend channel link with the client ID to the auth server)
4. user signs In
5. authorization server redirects to the BFF (with authorization code)
6. BFF exchanges authorization code for access and refresh token via client secret and stores it
7. BFF creates a session for the user and redirects the user back to the SPA where the session is stored, protected against CSRF and breach and in a HtpOnly Cookie with SameSite

# Client Credentials
- machine to machine communiacation
- usually client is not trying to get access token on behalf of an en user
- client usually wants to access its own data eg. backend request to other microservices, statistics like how many user are using an app etc..
- in this flow we still create an access token (and do not just send the client secret in each request), so other resource servers hav to only deal with one kind of token and also for security reasons
1. making a request to authorization server with `grant_type`: `client_credentials`, a scope, the client_id, client_secret like:
   ```
    POST https://authorization-server.com/oauth/token
		?client_id=CLIENT_ID
		&client_secret=CLIENT_SECRET
		&grant_type=client_credentials
		&scope=contacts
	```
2. the server validates the credentials and returns the token response (typically without a refresh token since the use case of a refresh token is to let the user refresh its access token without the whole authorization code flow, but here is no end user):
	```json
   {
	   "token_type": "Bearer",
	   "access_token": "XXXXXX",
	   "expires_in": 3600,
	   "scope": "photos",
	   "refresh_token": "XXXX"
   }
   ```