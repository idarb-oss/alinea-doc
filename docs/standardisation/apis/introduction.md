---
title: API Design Guide
summary: The standarisation of API creation.
authors:
  - Idar Bergli
date: 2021-04-06
---

Welcome to the API Design Guide for Alinea. This is heavly based on the [Google Design Guide](https://cloud.google.com/apis/design) from the beginning. But mainly to get a standarized structure. It will liv on and change as we continue to standarize the Alinea Platform.

List of latest changes can be found here:

[Changelog](changelog.md)

## Introduction

This is a general design guide for networked APIs. This design guide is shared here to inform outside developers and to make it easier for us all to work together.

This guide applies to both REST APIs but in the future we might also use if for RPC APIs.

This guide is a living document and additions to it will be made over time as new style and design patterns are adopted and approved. In that spirit, it is never going to be complete and there will always be ample room for the art and craft of API design.

## Conventions Used in This Guide

The requirement level keywords "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" used in this document are to be interpreted as described in [RFC 2119](https://www.ietf.org/rfc/rfc2119.txt).

In this document, such keywords are highlighted using bold font.
