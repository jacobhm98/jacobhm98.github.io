+++
title = 'MCP OAuth'
date = 2025-07-17T18:58:15+02:00
draft = true
+++

# The OAuth2.0 framework

Problem statement:

- Password auth is inherently insufficient in modern web applications
- Resource owners (users) want to be able to give access to third party applications tospecific resources they own without sharing their password, which would give the third party applications the same access rights as the resource owner
- They want to be able to revoke access from a specific third party application, or from a specific resource, without changing their root password
- A compromise in the third party application should not result in the malicious actor getting root access to all resources under ownership by the resource owner

Terms:

- Resource owner -- A user. I own various digital resource: email accounts, google drive files, slack accounts, etc...
- Resource server -- Typically a backend server. Gmail, google drive, slack, etc...
- Client -- An "instance" of the user. Slack acts as an oauth client when I try to connect it to various files in google drive (resource server).
- Authorization sever -- a server trusted by the resource server that the client can authenticate itself with. The client then shows the resource server an access token obtained by the authorization server to the resource server in order to gain access.

High leverl authorization flow:

Protocol Flow

     +--------+                               +---------------+
     |        |--(A)- Authorization Request ->|   Resource    |
     |        |                               |     Owner     |
     |        |<-(B)-- Authorization Grant ---|               |
     |        |                               +---------------+
     |        |
     |        |                               +---------------+
     |        |--(C)-- Authorization Grant -->| Authorization |
     | Client |                               |     Server    |
     |        |<-(D)----- Access Token -------|               |
     |        |                               +---------------+
     |        |
     |        |                               +---------------+
     |        |--(E)----- Access Token ------>|    Resource   |
     |        |                               |     Server    |
     |        |<-(F)--- Protected Resource ---|               |
     +--------+                               +---------------+

                     Figure 1: Abstract Protocol Flow

The abstract OAuth 2.0 flow illustrated in Figure 1 describes the
interaction between the four roles and includes the following steps:

(A) The client requests authorization from the resource owner. The
authorization request can be made directly to the resource owner
(as shown), or preferably indirectly via the authorization
server as an intermediary.

(B) The client receives an authorization grant, which is a
credential representing the resource owner's authorization,
expressed using one of four grant types defined in this
specification or using an extension grant type. The
authorization grant type depends on the method used by the
client to request authorization and the types supported by the
authorization server.

(C) The client requests an access token by authenticating with the
authorization server and presenting the authorization grant.

(D) The authorization server authenticates the client and validates
the authorization grant, and if valid, issues an access token.

(E) The client requests the protected resource from the resource
server and authenticates by presenting the access token.

(F) The resource server validates the access token, and if valid,
serves the request.

The preferred method for the client to obtain an authorization grant
from the resource owner (depicted in steps (A) and (B)) is to use the
authorization server as an intermediary, which is illustrated in
Figure 3 in Section 4.1.

Relevant RFC's

- https://datatracker.ietf.org/doc/html/rfc6749
- https://datatracker.ietf.org/doc/html/rfc7636
- https://datatracker.ietf.org/doc/html/rfc7591
- https://datatracker.ietf.org/doc/html/rfc8414
- https://datatracker.ietf.org/doc/html/rfc6819
- https://datatracker.ietf.org/doc/html/rfc6750
- https://datatracker.ietf.org/doc//html/rfc9700/
- https://datatracker.ietf.org/doc/html/rfc7662
- https://datatracker.ietf.org/doc/draft-ietf-oauth-v2-1/
