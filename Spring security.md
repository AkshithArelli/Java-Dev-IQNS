Spring Security is a powerful and customizable security framework used to secure Java/Spring applications.

✔ What it provides

	•	Authentication (who are you?)
	•	Authorization (what can you access?)
	•	Protection against:
	•	CSRF
	•	Session fixation
	•	Clickjacking
	•	CORS issues

✔ Why it’s widely used

	•	Integrates easily with Spring Boot
	•	Supports many authentication methods:
	•	Basic Auth
	•	Form Login
	•	Token-based login
	•	LDAP
	•	OAuth2
	•	JWT
	•	Highly customizable via filters

⸻

We majorly use two: 

1. Basic Auth
   
2. OAuth2

### Basic Auth

🎯 INTERVIEW ANSWER: How Basic Authentication works in Spring Security

I will break it into:

	1.	What is Basic Auth
  
	2.	How Spring handles it (filters + authentication manager)
  
	3.	When UserDetailsService is called
	
  4.	Why DaoAuthenticationProvider is used
	
  5.	Full code example
	
  6.	Flow diagram

⸻

✅ 1. What is Basic Authentication?

Basic Authentication means:

	•	Client sends credentials on every request
	•	Credentials are sent in the Authorization header

Authorization: Basic Base64(username:password)

Spring Security decodes and validates them.

⸻

✅ 2. How Spring Security handles Basic Auth internally

When you enable:

http.httpBasic();

Spring Security automatically registers:

1. BasicAuthenticationFilter
   
	•	Reads and decodes the Authorization header
	•	Extracts username/password
	•	Creates a token → sends to AuthenticationManager

3. AuthenticationManager (ProviderManager)
   
	•	Delegates authentication to providers

5. DaoAuthenticationProvider
   
	•	Default authentication provider for username/password
	•	Uses the UserDetailsService

7. UserDetailsService
   
	•	Loads user from database/in-memory
	•	Returns UserDetails object with username/password/roles

⸻

🔥 3. WHEN & WHY UserDetailsService is called

UserDetailsService is called during authentication inside DaoAuthenticationProvider.

Flow:

	1.	Filter extracts username/password
	2.	AuthenticationManager receives username/password
	3.	DaoAuthenticationProvider calls:

userDetailsService.loadUserByUsername(username)

	4.	It gets:
  
	•	stored password (encrypted)
	•	roles/authorities
  
	5.	DaoAuthenticationProvider compares:
```
passwordEncoder.matches(rawPassword, storedPassword)
```
If match → authentication success

⸻

🧠 4. Why DaoAuthenticationProvider is used?

Because:

	•	It is Spring Security’s default provider for username/password
	•	It automatically gets registered if a UserDetailsService bean exists
	•	Handles password checking, role loading, account locking, expiration, etc.

Important point interviewers expect:

Spring Security looks for a bean of type UserDetailsService, not the bean name.

If it finds one, it automatically creates a DaoAuthenticationProvider and wires it.

⸻

🟦 5. FULL WORKING EXAMPLE (Microservice → Another Microservice)

🟧 Microservice A → calling Microservice B using Basic Auth

Client Code inside Microservice A
```java
@RestController
public class OrderController {

    @Autowired
    private RestTemplate restTemplate;

    @GetMapping("/process")
    public String processOrder() {

        String username = "serviceA";
        String password = "password123";

        String base64Creds = Base64.getEncoder()
                                   .encodeToString((username + ":" + password).getBytes());

        HttpHeaders headers = new HttpHeaders();
        headers.add("Authorization", "Basic " + base64Creds);

        HttpEntity<String> entity = new HttpEntity<>(headers);

        ResponseEntity<String> response = restTemplate.exchange(
                "http://payment-service/pay",
                HttpMethod.POST,
                entity,
                String.class
        );

        return response.getBody();
    }
}
```

⸻

🟩 Microservice B (Payment Service) — Secured with Basic Auth

Security Configuration
```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(auth -> auth.anyRequest().authenticated())
            .httpBasic(); // enable Basic Auth

        return http.build();
    }

    @Bean
    public UserDetailsService userDetailsService(PasswordEncoder encoder) {
        UserDetails user = User.withUsername("serviceA")
            .password(encoder.encode("password123"))
            .roles("SERVICE")
            .build();

        return new InMemoryUserDetailsManager(user);
    }

    @Bean
    PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
}
```

⸻

🟩 Controller in Microservice B
```java
@RestController
public class PaymentController {

    @PostMapping("/pay")
    public String pay() {
        return "Payment Completed Successfully!";
    }
}
```

⸻

🔥 6. FULL INTERNAL AUTHENTICATION FLOW (Interview Diagram)
```
                    -----> BasicAuthenticationFilter
Incoming Request --->      Reads Authorization Header
                           Decodes Base64 Credentials
                           Creates UsernamePasswordAuthenticationToken
                                   |
                                   v
                       AuthenticationManager
                                   |
                                   v
                        DaoAuthenticationProvider
                                   |
                                   v
                  UserDetailsService.loadUserByUsername()
                                   |
         Gets user details from DB/in-memory (password + roles)
                                   |
                                   v
                    PasswordEncoder.matches()
                                   |
                                   v
              If match → Authentication Successful
                                   |
                                   v
                    SecurityContextHolder stores auth
                                   |
                                   v
                         Controller executes
```

⸻

🟣 FINAL INTERVIEW SUMMARY (Perfect to speak)

Basic Auth in Spring Security works through BasicAuthenticationFilter, which extracts Base64 credentials and sends them to the AuthenticationManager. The AuthenticationManager uses DaoAuthenticationProvider, which automatically wires the UserDetailsService bean. DaoAuthenticationProvider calls loadUserByUsername() to fetch stored user details and compares the raw password with the stored encrypted password using the PasswordEncoder. If they match, Spring sets the Authentication object in the SecurityContext and the controller method executes.

This is the EXACT answer an interviewer expects.

⸻

### OAuth2

	•	Authorization server
	•	Access tokens
	•	Refresh tokens
	•	How Microservice A calls Microservice B
	•	Exact REST API flow
	•	Real Java code examples
	•	What exactly happens inside Spring Security

This is designed so you can speak confidently in interviews.

⸻

⭐ SCENARIO

We have:

✔ Auth Service (Authorization Server)

Handles login, issues Access Token (JWT) and Refresh Token.

✔ Microservice A (Client)

Needs data from Microservice B.

First gets token from Auth Service → then calls B.

✔ Microservice B (Resource Server)

Has protected REST APIs, validates JWT.

⸻

🔥 1️⃣ USER + MICROSERVICE FLOW (INTERVIEW FRIENDLY)

Step 1 — Microservice A asks Auth Server for access token

This is typically using client credentials or password flow.

Example request:
```
POST http://auth-service/oauth/token
Content-Type: application/json

{
  "clientId": "microA",
  "clientSecret": "abc123",
  "grant_type": "client_credentials"
}
```
Auth-Service does:

	•	Validates clientId & secret
	•	Issues Access Token (JWT)
	•	Issues Refresh Token (optional depending on flow)

Response example:
```
{
  "access_token": "eyJhbGciOiJIUzI1NiIs...",
  "refresh_token": "eyJhbGciOiJIUz...",
  "expires_in": 3600
}
```

⸻

🔥 2️⃣ Microservice A calls Microservice B WITH ACCESS TOKEN
```
GET http://microservice-b/api/products
Authorization: Bearer eyJhbGciOiJIUzI1NiIs...
```

⸻

🔥 3️⃣ Microservice B receives request → Spring Security handles it

Inside B:

	•	BearerTokenAuthenticationFilter runs
	•	Extracts JWT
	•	Delegates to JwtAuthenticationProvider
	•	JWT is validated with public key / secret key
	•	If valid → API is executed
	•	If invalid → 401 Unauthorized

⸻

🔥 4️⃣ Token Expired? → Microservice A requests new Access Token via Refresh Token

Request:
```
POST http://auth-service/oauth/token
{
  "grant_type": "refresh_token",
  "refresh_token": "eyJhbGciOiJIUz..."
}
```
Auth-Service returns:
```
{
  "access_token": "new_access_token",
  "refresh_token": "new_refresh_token"
}
```

⸻

🔥 5️⃣ Microservice A retries the API call with new access token

⸻

⭐ NOW LET’S SEE THE INTERNAL SPRING SECURITY FLOW (Interview gold)

This is the cleanest way to explain in interviews:

⸻

✔ OAuth2 Authentication Flow in Spring Security

1. Request hits Security Filter Chain

Spring Security adds:

	•	BearerTokenAuthenticationFilter
	•	AuthorizationFilter

2. Bearer filter extracts JWT token
```
Authorization: Bearer <jwt>
```
3. Filter calls AuthenticationManager
```
authenticationManager.authenticate(jwtAuthenticationToken)
```
4. AuthenticationProvider validates JWT
```
Provider = JwtAuthenticationProvider
```
It checks:

	•	Signature
	•	Expiry
	•	Issuer
	•	Audience
	•	Roles (claims)

5. If valid → SecurityContext populated

SecurityContextHolder.getContext().setAuthentication(authentication)

Now controller methods can use @PreAuthorize, @AuthenticationPrincipal, etc.

⸻

⭐ CODE EXAMPLES (REAL MICROSERVICE CODE)

⸻

1️⃣ MICROSERVICE A — Calling Auth Service
```java
RestTemplate rest = new RestTemplate();

Map<String, String> body = new HashMap<>();
body.put("clientId", "microA");
body.put("clientSecret", "abc123");
body.put("grant_type", "client_credentials");

ResponseEntity<Map> response =
    rest.postForEntity("http://auth-service/oauth/token", body, Map.class);

String accessToken = (String) response.getBody().get("access_token");
```

⸻

2️⃣ MICROSERVICE A — Calling Microservice B with token
```java
HttpHeaders headers = new HttpHeaders();
headers.add("Authorization", "Bearer " + accessToken);

HttpEntity<Void> request = new HttpEntity<>(headers);

String response = rest.exchange(
    "http://microservice-b/api/products",
    HttpMethod.GET,
    request,
    String.class
).getBody();
```

⸻

3️⃣ MICROSERVICE B — Security Configuration
```java
@Configuration
public class SecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {

        http.csrf().disable()
            .authorizeHttpRequests(auth -> auth
                .anyRequest().authenticated()
            )
            .oauth2ResourceServer(oauth ->
                oauth.jwt()   // enables JWT validation
            );

        return http.build();
    }
}
```

⸻

4️⃣ MICROSERVICE B — Public Key to validate JWT
```
spring.security.oauth2.resourceserver.jwt.jwk-set-uri=http://auth-service/oauth/.well-known/jwks.json
```
This tells B how to validate JWT signature.

⸻

5️⃣ AUTH SERVICE — issuing JWT token
```java
@Service
public class TokenService {

    public String generateToken(String clientId) {
        return Jwts.builder()
            .setSubject(clientId)
            .claim("roles", List.of("SERVICE"))
            .setIssuedAt(new Date())
            .setExpiration(new Date(System.currentTimeMillis() + 3600 * 1000))
            .signWith(Keys.hmacShaKeyFor(secret.getBytes()))
            .compact();
    }
}
```

⸻

⭐ FULL END-TO-END SUMMARY (What you say in interview)

OAuth2 is an authorization framework where an Authorization Server issues Access Tokens (JWTs). In microservices, one service (A) first requests an access token from the Auth Server using either client credentials or refresh token flow. Then A calls service B by sending it in the Authorization header as “Bearer ”. B acts as a Resource Server and validates the JWT using the Authorization Server’s public key. If the token is valid, B executes the API; otherwise it returns 401. Refresh tokens ensure long-lived authentication without sending credentials again.

Note: client credentials vs refresh token (when to use)

When services talk to services →

Client ID & Secret

Like service’s username and password.

When users talk to systems →

Refresh Token

Like a user’s long-term login cookie.


⸻

### 1. What Does an Access Token Contain? (Interview Answer)

“An access token—usually a JWT—contains all the information a microservice needs to identify the user and authorize the request, without calling any database.”

A JWT access token has three main parts:

⸻

🔹 1. Header (Metadata)

Contains:

	•	Algorithm used to sign JWT (HS256 / RS256)
	•	Token type (JWT)

Example:

{
  "alg": "RS256",
  "typ": "JWT"
}


⸻

🔹 2. Payload (Claims) — This is the main part

Contains details about the user and token:

Standard claims
```
Claim			Meaning
sub				User ID (“subject”)
exp				Expiry time
iat				Issued time
iss				Issuer (Auth server)
aud				Audience (which services can use this token)
```
Custom claims
```
Claim	Meaning
roles	User roles → [“ADMIN”, “USER”]
scope	Permissions → “read write”
email	Email (optional)
```

⸻

🔹 3. Signature

The most important part.

	•	Generated using header + payload + private key
	•	Prevents tampering
	•	Ensures token is authentic, issued by your auth server

⸻

🎯 Summary (Use this sentence in interview)

“A JWT access token contains the user identity, roles, permissions, expiry time, issuer, audience, and a digital signature to prevent tampering.”

⸻

### 2. How Microservice B Validates the Access Token (Interview Answer)

When Microservice A calls Microservice B:

Authorization: Bearer <access_token>

Microservice B uses Spring Security or a JWT filter to validate the token in these steps:

⸻

Step 1 — Extract Token

From Authorization header.

⸻

Step 2 — Validate Signature

Microservice B uses the public key (RS256) or shared secret (HS256) to check:

	•	Is this token signed by our auth server?
	•	Has it been modified?

If signature invalid → 401 Unauthorized

This prevents hackers from editing roles or expiry.

⸻

Step 3 — Validate Expiry

Check exp (expiry claim):

	•	If expired → reject token (401)

⸻

Step 4 — Validate Issuer

Check iss claim:

iss must equal https://auth.mycompany.com

Reject if token is issued by unknown server.

⸻

Step 5 — Validate Audience

Check aud claim:

aud must contain "microservice-B"

Ensures the token is actually meant for this service.

⸻

Step 6 — Extract User Information

From payload:

	•	userId = payload.sub
	•	roles = payload.roles
	•	scope = payload.scope

These are used to build a SecurityContext in Spring.

⸻

Step 7 — Authorize Request

Microservice B checks:

	•	Is this endpoint allowed for the roles?
	•	Does scope allow this operation?

Example:
```
/admin → only ADMIN role
/orders → must have scope orders.read
```

⸻

🎯 Summary (Use this sentence in interview)

“Microservice B validates the access token by verifying its signature, expiry, issuer, audience, and then extracting claims like user ID and roles to authorize the request.”

⸻

⭐ Final Short Interview Answer

Q: What does an access token contain and how is it validated?

A:
“An access token, usually a JWT, contains user identity (sub), roles, permissions (scope), expiry time (exp), issuer (iss), audience (aud), and a digital signature.
Microservice B validates the token by checking the signature to ensure it wasn’t tampered with, verifying expiry, issuer, and audience, and then reading claims from the payload to authorize the request. No database lookup is needed because JWT is self-contained.”


