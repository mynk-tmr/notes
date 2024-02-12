## JSON Web Token (JWT) 

A *stateless* method of securely transmitting info as JSON. Stateless means each request is *self-contained* and can be used to authenticate user without having to store session data.
##### JWT parts
- A *Header* : token type and hash algo used
- A *Payload* : auth verification data (like `userID` in database) // script can't see
- A *Signature* :  to verify token’s integrity (output of `jwt.sign`)
##### JWT authentication working
- *Token Generation*: server generates a JSON token identifying user and auth session (like expiry date)
- *Token Storage:*  client stores that as cookie (or in local storage)
- *User Verification*: In a request, client sends **stored jwt** in headers (`credentials: include`) to server to get access to protected resources
- *Token Expiration:* now re-login to obtain new JWT
##### Best practices
- when to use: stateless api, microservices, reducing database calls
- when NOT: payment related data (credit card no.) should never be sent in payload
- short expiration, `http-only, secure`
- Implement Token Revocation for compromised ones