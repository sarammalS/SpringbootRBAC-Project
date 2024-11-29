# SpringbootRBAC-Project
# 1. Authentication System in  Project
# 1.1 User Registration
Implemented via the /signup endpoint in the AuthController.
Users register with:
Username: Checked for uniqueness using userRepository.existsByUsername().
Email: Checked for uniqueness using userRepository.existsByEmail().
Password: Securely hashed using PasswordEncoder.
Roles: Assigned during registration (default is ROLE_USER if no role is specified).
# 1.2 User Login
Implemented via the /signin endpoint in the AuthController.
Users provide credentials (username, password) which are authenticated using AuthenticationManager.
On successful authentication:
A JWT token is generated using JwtUtils.generateJwtToken().
The JWT token contains the user's details and roles.
 # 1.3 User Logout
Not explicitly implemented but can be extended by:
Adding a logout endpoint to blacklist tokens.
Using a short-lived JWT strategy.
# 2. Authorization System in  Project
2.1 Role-Based Access Control (RBAC)
RBAC is implemented using roles and authorities:
Roles are defined in the ERole enum (e.g., ROLE_ADMIN, ROLE_USER, ROLE_MODERATOR).
Each user has a Set<Role> that determines their permissions.
During login, roles are extracted and included in the JWT token.
# 2.2 Role Assignment
Role assignment happens during registration:
Default role: ROLE_USER.
Other roles (ROLE_ADMIN, ROLE_MODERATOR) can be specified in the SignupRequest.
# 2.3 Secured API Endpoints
Endpoints can be secured using role checks:
Example: Only users with the ROLE_ADMIN role can access admin-specific endpoints.
This is handled using Spring Security annotations or custom filters.
# 3. JWT Usage
JWT Generation: Implemented in JwtUtils.generateJwtToken().
JWT Parsing and Validation:
Tokens are validated using JwtUtils.validateJwtToken().
User details are extracted from the token and set in the security context.
# 4. Database Design for RBAC
 database already supports roles using the Role entity.
Ensure a many-to-many relationship between User and Role is implemented.
Future Improvements
Token Blacklisting:
Implement a blacklist mechanism to invalidate JWTs during logout.
Role-Permission Mapping:
Add a Permission entity to allow granular control over actions.
OAuth 2.0 Integration:
Consider adding OAuth 2.0 for social login or third-party integrations.
# END POINTS:
Authentication Endpoints
# User Login

Endpoint: /api/auth/signin

Method: POST

Purpose: Authenticate the user and generate a JWT token.
# User Signup
Endpoint: /api/auth/signup

Method: POST

Purpose: Register a new user with roles.

Secured Endpoints
# Public Access

Endpoint: /api/test/all

Method: GET

Purpose: Publicly accessible endpoint.
# User Access

Endpoint: /api/test/user

Method: GET

Purpose: Secured endpoint for authenticated users with the ROLE_USER.
# Moderator Access

Endpoint: /api/test/mod

Method: GET

Purpose: Secured endpoint for authenticated users with the ROLE_MODERATOR.
# Admin Access

Endpoint: /api/test/admin

Method: GET

Purpose: Secured endpoint for authenticated users with the ROLE_ADMIN.
