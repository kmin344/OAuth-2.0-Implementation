# OAuth 2.0 Implementation for Microservices Architecture
<img width="1033" alt="image" src="https://github.com/user-attachments/assets/152ce7c4-470a-453c-af8a-7fcf7ef8e87e">

## Project Overview
Developed an OAuth 2.0 authentication system to address the lack of shared login authentication in a new Microservices Architecture (MSA). The system issues tokens for use across MSA components when users log in to the main system.

## Architecture
```mermaid
graph TD
    A[Client Browser] -->|1. HTTPS Login Request| B(Main System)
    B -->|2. Redirect to Auth| C[Accounts Server / Auth Service]
    C -->|3. HTTPS Login Page| A
    A -->|4. HTTPS Credentials| C
    C -->|5. One-time Code| A
    A -->|6. Code| B
    B -->|7. Code + Client Credentials| C
    C -->|8. Access Token + Refresh Token| B
    B -->|9. Set Tokens in Secure Cookies| A

    A -->|10. HTTPS API Request + Token| D{API Gateway}
    D -->|Validate Token| C
    D -->|11. Authorized Request| E[Microservice 1]
    D -->|11. Authorized Request| F[Microservice 2]
    D -->|11. Authorized Request| G[Microservice n]

    A -->|12. Logout Request| B
    B -->|13. Invalidate Token| C

    H[Token Refresh Process] -->|When Token Expires| C
    C -->|New Access Token| A

    I[Error Handling] -->|401 Unauthorized| A

    subgraph MSA
    D
    E
    F
    G
    end

    subgraph Security Measures
    J[HTTPS]
    K[Secure Cookies]
    L[Token Invalidation]
    end

    style MSA fill:#f9f,stroke:#333,stroke-width:2px
    style Security Measures fill:#ff9,stroke:#333,stroke-width:2px
```

## Key Challenges
- Absence of shared login authentication between main codebase and new MSA structure
- Need for a token system usable across multiple microservices

## Technical Implementation
- Registered applications on the accounts server to issue APIs
- Implemented one-time code issuance when customers log in via their browser
- Developed a process to exchange the one-time code for an access token, which is then used for API requests

## Achievements
- Enabled independent development and deployment of MSAs by multiple squads in a multi-team service operation environment
- Implemented a system where a single customer login event grants all necessary permissions for MSA operations

## Technologies Used
- OAuth 2.0
- API development
- Token-based authentication
- Microservices Architecture (MSA)
