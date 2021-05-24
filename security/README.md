# Security Considerdations When Designing a System
##### Least Privilege
##### Fail Safe Defaults
- Negate fail safe insecure defaults
##### Economy of Mechanisms ( Keep it Simple )
##### Complete Mediation
- Check access to each abject is allowed
- Note cache machanisms. After access is revoked, is access verified against stale cache data?
##### Open Design
- Security of the design should not depend on the secrecy of the design. 
##### Seperation of Privilege
- Permissions based on more than one condition. Just because someone has a password, can they use it to accomplish a specific task?
##### Least Common Mechanism
- Do not create shared resources with sensitive data.
##### Psychological Acceptability
- The more secure a design is, the more likely users are to find ways to make life easier by creating insecure work arounds. 