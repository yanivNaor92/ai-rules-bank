# Unified Gateway Overview
The Unified Gateway service (UGW) is a centrally managed ingress layer that serves as an entry point for all SAP Cloud solutions. It simplifies the exposure and consumption of applications and APIs, and routes all external traffic to the appropriate Line of Business (LoB) service.<br>
Customer and partner clients can establish a connection to Unified Gateway that is called a downstream connection.<br>
Unified Gateway establishes a connection to the LoB endpoint that is called an upstream connection.

Unified Gateway provides the following benefits:

- Enables SAP to observe API traffic and monitor API for monetization, Service Level Agreement (SLA), compliance, and more (via integration with API Insights)
- Provides consistent consumption of applications and APIs for all SAP Cloud solutions  (for example, common super domain, user identity, protocol, and more)
- Offloads standard capabilities to reduce load on LoBs (for example, rate limiting and JWT validation). In future releases, the following capabilities will be added: DDoS protection, CSRF token handling, web application firewall, and more.
- Streamlines and enforces SAP standards and guidelines (for example, security standards) across LoBs
- Enables routing to carved-out services (Strangler pattern)

Furthermore, using the service means reduced total costs for development and operations by avoiding redundancies. It also provides transparency of quality and characteristics due to the explicit contracts between applications/services and Unified Gateway.