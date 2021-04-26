# Azure API Experiment

We have an interactive website which needs placing behind authentication, with the exception of one
endpoint which needs to be publically accessible.

How do we host this in a cost-effective manner within Azure?

## Constraints

- The web application is authenticated with Azure AD
- The endpoint can be called externally from a web hook EITHER with a long-lived token OR with no token at all
- The expense of hosting is minimal (this means no VPC, API Gateway etc)

# Setup

- Basic API
- Container
- Azure hosting

## Basic API

An aspnet core web API which exposes two endpoints:
- `/` (intended to be behind some form of auth)
- `/open` (intended to be openly accessible)

See `SplitWebsite/Startup.cs` for the routing.

## Container Images

### 1. Split Website

Container definition for `SplitWebsite` will build and make available the webAPI without authentication.
It is currently built and provided as an open project here: https://hub.docker.com/repository/docker/jonathangilroypowell/basic-aspnet-webapp/general

To trigger another build and release, create an annotation tag on the master branch with an incrementing version, e.g.
```
git checkout master
git tag -a 1.0.6 -m "New version"
git push origin 1.0.6
```

### 2. oauth2-proxy

A reverse proxy that provides OAuth2 functionality `bitnami/oauth2-proxy`

The configuration of the oauth2 proxy must:
- forward all calls to our aspnet web site
- authenticate requests to all routes EXCEPT those explicitly identified, e.g. `/open`


## Azure hosting

The Azure hosting consists of
- resource group ``
- a linux app service plan ``
- a web app within the plan
-- using the container image built previously
-- configured with SSL
- Azure AD
-- an App Registration for our site
-- a user, I guess?