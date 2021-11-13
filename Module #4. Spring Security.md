# Module #4. Spring Security

### What is Spring Security?

Spring Security is a framework that provides authentication, authorization, and protection against common attacks.

### Main principles of Spring Security. How does security work internally?

Application security boils down to two more or less independent problems: authentication (who are you?) and authorization (what are you allowed to do?). Sometimes people say “access control” instead of "authorization", which can get confusing, but it can be helpful to think of it that way because “authorization” is overloaded in other places. Spring Security has an architecture that is designed to separate authentication from authorization and has strategies and extension points for both.

#### **Authentication**

The main strategy interface for authentication is `AuthenticationManager`, which has only one method:

```java
public interface AuthenticationManager {

  Authentication authenticate(Authentication authentication)
    throws AuthenticationException;
}
```

An `AuthenticationManager` can do one of 3 things in its `authenticate()` method:

- Return an `Authentication` (normally with `authenticated=true`) if it can verify that the input represents a valid principal.
- Throw an `AuthenticationException` if it believes that the input represents an invalid principal.
- Return `null` if it cannot decide.

#### **Authorization or Access Control**

Once authentication is successful, we can move on to authorization, and the core strategy here is `AccessDecisionManager`.

#### How does security work internally?

Spring Security in the web tier (for UIs and HTTP back ends) is based on Servlet `Filters`.

Spring Security is installed as a single `Filter` in the chain, and its concrete type is `FilterChainProxy`, for reasons that we cover soon. In a Spring Boot application, the security filter is a `@Bean` in the `ApplicationContext`, and it is installed by default so that it is applied to every request. It is installed at a position defined by `SecurityProperties.DEFAULT_FILTER_ORDER`, which in turn is anchored by `FilterRegistrationBean.REQUEST_WRAPPER_FILTER_MAX_ORDER`(the maximum order that a Spring Boot application expects filters to have if they wrap the request, modifying its behavior). There is more to it than that, though: From the point of view of the container, Spring Security is a single filter, but, inside of it, there are additional filters, each playing a special role. The following image shows this relationship:

![https://github.com/spring-guides/top-spring-security-architecture/raw/main/images/security-filters.png](https://github.com/spring-guides/top-spring-security-architecture/raw/main/images/security-filters.png)

### What does authentication and Authorization mean? What is deference?

**Authentication** is the process of verifying who someone is, whereas **authorization** is the process of verifying what specific applications, files, and data a user has access to.

### What is Oauth2? What is Authorization Server, Resource Server? How to get oauth token?

[OAuth 2.0](https://tools.ietf.org/html/rfc6749), which stands for “Open Authorization”, is a standard designed to allow a website or application to access resources hosted by other web apps on behalf of a user. It replaced OAuth 1.0 in 2012 and is now the de facto industry standard for online authorization. OAuth 2.0 provides consented access and restricts actions of what the client app can perform on resources on behalf of the user, without ever sharing the user's credentials.

#### **OAuth2.0 Roles**

- **Resource Owner**: The user or system that owns the protected resources and can grant access to them.
- **Client**: The client is the system that requires access to the protected resources. To access resources, the Client must hold the appropriate Access Token.
- **Authorization Server**: This server receives requests from the Client for Access Tokens and issues them upon successful authentication and consent by the Resource Owner. The authorization server exposes two endpoints: the Authorization endpoint, which handles the interactive authentication and consent of the user, and the Token endpoint, which is involved in a machine to machine interaction.
- **Resource Server**: A server that protects the user’s resources and receives access requests from the Client. It accepts and validates an Access Token from the Client and returns the appropriate resources to it.

### What is OpenId Connect?

OpenID Connect 1.0 is a simple identity layer on top of the OAuth 2.0 protocol. It allows Clients to verify the identity of the End-User based on the authentication performed by an Authorization Server, as well as to obtain basic profile information about the End-User in an interoperable and REST-like manner.

OpenID Connect allows clients of all types, including Web-based, mobile, and JavaScript clients, to request and receive information about authenticated sessions and end-users. The specification suite is extensible, allowing participants to use optional features such as encryption of identity data, discovery of OpenID Providers, and session management, when it makes sense for them.

### What is a Security context?

The **SecurityContext** and **SecurityContextHolder** are two fundamental classes of Spring Security. The `SecurityContext` is used to store the details of the currently authenticated user, also known as a principle.

### What is Security principal?

The principal is **the currently logged in user**.

### What is JWT? Describe JWT structure?

A JWT is a structured security token format used to encode JSON data. The main reason to use JWT is to exchange JSON data in a way that can be cryptographically verified. There are two types of JWTs:

- **J**SON **W**eb **S**ignature (JWS)
- **J**SON **W**eb **E**ncryption (JWE)

The data in a JWS is public—meaning anyone with the token can read the data—whereas a JWE is encrypted and private. To read data contained within a JWE, you need both the token and a secret key.

When you use a JWT, it’s usually a JWS. The 'S' (the signature) is the important part and allows the token to be validated. For the rest of this post, I will talk about the JWS format and walk through decoding an example JWT.

A JWS (the most common type of JWT) contains three parts separated by a dot (`.`). The first two parts (the "header" and "payload") are Base64-URL encoded JSON, and the third is a cryptographic signature.

#### Structure

Let’s look at an example JWT:

```
eyJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiSm9lIENvZGVyIn0.5dlp7GmziL2QS06sZgK4mtaqv0_xX4oFUuTDh1zHK4U
```

Breaking this down into the individual sections we have:

```
eyJhbGciOiJIUzI1NiJ9 *# header*.
eyJuYW1lIjoiSm9lIENvZGVyIn0 *# payload*.
5dlp7GmziL2QS06sZgK4mtaqv0_xX4oFUuTDh1zHK4U *#signature*
```

Next, each of the first two sections are base64-url decoded:

```
{
	"alg":"HS256" # header
} 
.
{
	"name":"Joe Coder" # payload
} 
.
5dlp7GmziL2QS06sZgK4mtaqv0_xX4oFUuTDh1zHK4U #signature
```

#### JWT Claims

Once you start using JWTs you start hearing the word "claim" everywhere. A JWT claim is a key/value pair in a JSON object. In the example above, `"name": "Joe Coder"`, the claim key is `name` and the value is `Joe Coder`. The value of a claim can be any JSON object.

### Why should you use JWT?

JWT is a particularly useful technology for API authentication and server-to-server authorization. 

Instead of storing information on the server after authentication, JWT creates a JSON web token and encodes, sterilizes, and adds a signature with a secret key that cannot be tampered with. This key is then sent back to the browser. Each time a request is sent, it verifies and sends the response back.

The user’s state is not stored on the server, as the state is instead stored inside the token on the client-side.

### Is it possible to achieve stateless requests using JWT?

JWT authentication/authorization is stateless in its essence. The user’s state is not stored on the server, as the state is instead stored inside the token on the client-side.

### What is CSRF protection, why is it needed? What is the purpose of the CSRF attack? How to protect application from CSRF?

**Cross-site request forgery (also known as CSRF)** is a web security vulnerability that allows an attacker to induce users to perform actions that they do not intend to perform. It allows an attacker to partly circumvent the same origin policy, which is designed to prevent different websites from interfering with each other.

In a successful CSRF attack, the attacker causes the victim user to carry out an action unintentionally. For example, this might be to change the email address on their account, to change their password, or to make a funds transfer.

The most robust way to defend against CSRF attacks is to include a **CSRF token** within relevant requests.

A **CSRF token** is a unique, secret, unpredictable value that is generated by the server-side application and transmitted to the client in such a way that it is included in a subsequent HTTP request made by the client. When the later request is made, the server-side application validates that the request includes the expected token and rejects the request if the token is missing or invalid.

CSRF tokens can prevent CSRF attacks by making it impossible for an attacker to construct a fully valid HTTP request suitable for feeding to a victim user. Since the attacker cannot determine or predict the value of a user's CSRF token, they cannot construct a request with all the parameters that are necessary for the application to honor the request.

CSRF tokens should be treated as secrets and handled in a secure manner throughout their lifecycle. An approach that is normally effective is to transmit the token to the client within a hidden field of an HTML form that is submitted using the POST method. The token will then be included as a request parameter when the form is submitted:

```html
<input type="hidden" name="csrf-token" value="CIwNZNlR4XbisJF39I8yWnWX9wX4WFoz" />
```