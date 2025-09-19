# JSON Web Token (JWT)

## What is JWT?
- JWT is a secure way to send information (like user ID or roles) between a client and server as a digitally signed token.
- Mainly used for authentication (logging in users) and authorization (giving access to resources)
- structure of JWT: xxx.yyy.zzz

## structure of JWT explained
1. Header - contains metadata about the token (like algorithm, type)
2. Paylod - contains user data like (id, email, role)
3. Signature - verifies the token hasn't been changed

## Example of decoded JWT 
Header: {
  "alg": "HS256",
  "typ": "JWT"
}

Payload: {
  "id": "12345",
  "username": "sumit",
  "role": "user"
}

Signature: hash of (header + payload + secret)

## How JWT works (Authentication flow)
1. User log in -> sends username + password
2. Server verifies credentials -> if valid, it creates a JWT signed with a secret key
3. Client stores JWT -> in localStorage, sessionStorage, or httpOnly cookies
4. Client makes requests -> sends JWT in request headers (usually Authorization: Bearer <token>)
5. Server verifies JWT -> if valid, it gives access to protected resources

## JWT Advantages and Disadvantages
### Advantages
- Stateless (server )



