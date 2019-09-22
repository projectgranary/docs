---
description: >-
  This document provides background information and guides for setting up
  role-based access control (RBAC) on a Granary instance.
---

# Identity and Access Management

## Introduction

Granary offers a range of customizable interfaces to restrict the API access to profile, event, and segment data using [Keycloak](https://www.keycloak.org/about.html) as the main interface. Keycloak is a Open Source Identity and Access Management \(IAM\) server by JBoss. It can be configured either through an UI or an imported JSON. 

In Granary, Keycloak acts, on the one hand, as a provider for a security realm, clients, groups, roles, and technical users. On the other hand, it acts as a proxy to authentication human users against an external identity provider such as a corporate Active Directory or LDAP. User-Group assignments of such external identity providers are mapped to the groups defined in Keycloak. By this means, a costumer can fully integrate fine-grained access control to data residing in Granary within its own identity management infrastructure.

## IAM Architecture Chart

![Functional Overview of IAM Architecture](../.gitbook/assets/image%20%282%29.png)

## Interfaces

The interfaces that require IAM definitions are driven by [Granary's dataflow](../developer-reference/dataflow/).

* **Belt Extractor**
  * require technical users to read profiles
* **Profile Store API**
  * requires roles to access a profile's Grains
* **Event Store API**
  * requires roles to access typed raw events from certain sources
* **Segment Store API**
  * requires technical users to access segment tables for OLAP queries requires roles to access specific segment tables
* **Groups**
  * are required to map human or technical users to certain roles



