# Multen Operator

The Multen Operator is a Kubernetes operator which provides secure multi-tenant access to an application via an authentication provider and separated data storage. **Currently the only providers are Envoy Gateway + Keycloak for authentication and PGO for data storage**

## Dependencies

This project would not be possible without the following awesome open-source projects:

* operator-sdk
* CrunchyData's Postgres Operator
* Envoy Gateway
* Keycloak