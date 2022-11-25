---
title: Security in machine to machine interaction
---
Integration between different machines over a communication medium, such as APIs, events or files

Principles:
* No trust - Don't trust the channel. Don't trust the transmitter. Don't trust the reciver.
* Follow the rules of privacy and make sure that communication is traceable, linkable and identifiable.

## DNS
DNS is crucial/weak/important

The DNS is responsible for the mapping between domain names and IP adresses.
The domain name is the public identity of the server. 

Design
* Use DNSsec for authenticating DNS records 
* Consider HA over failover
* Have zoning and multiple topologies

## TLS
TLS is a protocol for channel security. 
Correctly configured enables verification of the server via the domain name.

Trust anchors
* DNS 
* CA (certificate authority).

Weakness
* Options, variants, versions and configurations

Best practice
* Always use TLS (almost always anyway)
* Use secure protocols and ciphers
* Use a public trusted CA
* Secure the DNS (with DNSsec etc)
* Use forward secrecy
* Use full certificate chains
* Strong private key
* Secure private key
* Disable insecure renegotiation

Links
* [SSL labs](https://github.com/ssllabs/research/wiki/SSL-and-TLS-Deployment-Best-Practices)

## API - REST, SOAP, GRPC etc.
Principles:
* Log calls and standardise logging 
* Use a positive security model and deny by default
* Don't trust user input, have whitelists and disable insecure serializers.
* Use API gateways to minimize attack surface and consolidate logging and traffic inspections.
* Use TLS and consider mTLS for sensitive information.

### Authentication
* Check all possible ways to authenticate to all APIs
* Use standard authentication, token generation, password storage,
Multi-factor 
* Use short-lived access tokens
* Authenticate your apps (so you know who is talking to you)
* Use stricter rate-limiting for authentication, implement lockout
policies and weak password checks

### Authorization
* Implement authorization checks with user policies and hierarchy • Don’t rely on IDs sent from client. Use IDs stored in the session
object instead.
* Check authorization each time there is a client request to
access database
* Use random non-guessable IDs (UUIDs)

### Logging 
* Log failed attempts, denied access, input validation failures, any failures in security policy 
* Ensure that logs are formatted to be consumable by other tools
* Protect logs as highly sensitive
* Include enough detail to identify attackers
* Avoid having sensitive data in logs - If you need the information for 
debugging purposes, redact it partially.
* Integrate with SIEMs and other dashboards, monitoring, alerting
tools

### Data exposure
* Never rely on client to filter data
* Review all responses and adapt responses to what the API
consumers really need
* Define schemas of all the API responses
* Don’t forget about error responses
* Identify all the sensitive or PII info and justify its use
* Enforce response checks to prevent accidental data and exception
leaks
    
## File transfer (FTP etc)
* Use sFTP with certificates.
* Encrypt and sign sensitive files, exchange keys out of band.
* Have strict access permissions.
* Use disk encryption
* Logg activities

## Email
* Avoid emails especially for sensitive information.
* Educate users to create awareness of phishing and other types of treats 
* Apply MFA to prevent account takeover
* Use an email gateway that has Security capabilities usch as spam filtering, malware scanning and monitoring 
* Use DMARC to prevent domain fraud.
* Use TLS (StarTLS) and DNS-based Authentication of Named Entities (DANE) for transport security.
* Apply secure data transfer with trusted parties e.g setting up S/MIME 

Links
* [Email principles](https://explained-from-first-principles.com/email/)
