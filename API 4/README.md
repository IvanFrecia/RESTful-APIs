*Flask-jwt-extended Section
This API presents the basic usage of an active flask JWT extension called flask-jwt-extended. Inherited and simplified the project structure from API 3 to demonstrate how to apply flask-jwt-extended to this project.*

*Features

JWT authentication
Token refreshing
Fresh token vs. Non-fresh token
Adding claims to JWT payload
Blacklist and token revoking
Customize JWT response/error message callbacks
JWT authentication
Very much the same as before, but defined a UserLogin resource ourselves, as opposed to having a /auth endpoint created internally. And we no longer need the security.py file consequently.

*Token Refreshing

It allows the user login without having to entering the username and password over and over again. When logging in, we get an access token as before, and we also get a refresh token. We can use this refresh token to generate new access tokens without enter user credentials when the current access token expires.

Fresh token vs. Non-fresh token
When generating an access token, you can pass in an optional boolean argument fresh. We usually want to distinguish if the token is generated via credentials or via refresh token. Since the one generated with credentials should be considered more secure. So what we did in the project is to return a fresh access token and a refresh token from the /login endpoint, since user would need to provide their credentials to authenticate through /login. As for the /refresh endpoint, it only requires a refresh token and no credentials, so it would return a non-fresh access token. The non-fresh access token still gives us some belief that it's the user himself, but it is not that secure. So it's okay to allow the user to access most endpoints using a non-fresh token, but we may want the user to input his credentials again (to get a fresh access token) when performing some more important actions, such as deleting things or payment.

*Adding claims to JWT payload

Can also add some extra data to each JWT payload, and these data are accessible within the endpoint. We refer to these data as claims. We can attach any claims we find necessary to the JWT payload. In this section, we showed one use case, where we check if the authenticated user is an admin, and added a boolean claim is_admin to the payload. Then in each JWT-protected endpoint, we can tell if the user is ad admin, and decide what actions should be taken from there.

*Blacklist and token revoking

We can blacklist a user or revoke a token (access token or refresh token) and disable the user from logging in and accessing a protected endpoint. Possible use cases for this feature include: if a user complains his account being compromised, or we decide to take down an existing account temporarily, we can blacklist the user and revoke the token thus prevent the user from logging in.

*Customize JWT messages

The default response/error handler may not have the nicest format of output. It uses clueless abbreviation such as iat for issue at time and msg for message. We can customize the callbacks and display the message we felt more readable.

*Related Resources

UserLogin
POST: /login
Description: authenticate a user and ,if authenticated, respond with a fresh access token and a refresh token.
TokenRefresh
POST: /refresh
Description: This endpoint is used to refresh an expired access token. If the refresh token is valid, respond with a new valid access token (non-fresh).
Request header: Authorization Bearer <refresh_token>
Item
GET: /item/<name>
Description: get an item by name, require a valid access token to access this endpoint.
Request header: Authorization Bearer <access_token>
POST: /item/<name>
Description: create a new item, require a valid and fresh access token to access this endpoint.
Request header: Authorization Bearer <access_token>
DELETE: /item/<name>
Description: delete an item by name, require a valid access token and admin privilege.
Request header: Authorization Bearer <access_token>
ItemList
GET: /items
Description: get all items, half protected. User can get more detailed info when providing an access token.
Request header: Authorization Bearer <access_token>
Suggested introduction order
