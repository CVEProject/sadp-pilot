# Supplier ADP (SADP) Pilot

## Summary

This repository documents the Supplier-as-ADP Pilot project. This README and other documentation should be considered works-in-progress and are likely to be incomplete.

Two work-in-progress documents:

* [Supplier-CNA-as-ADP Pilot Overview](https://docs.google.com/document/d/1JLF5vj_8W7KVeqJNhQl8Fj7PLv-ujbqMLTU6LtDlmvI)
* [Supplier-CNA-as-ADP Pilot](https://docs.google.com/document/d/1u2qKAYo7AYOCDYVv1Le58kv5YpzHBsrwuib7OPY-YyU)

[Slides](https://youtu.be/dBtlUz0kUww?si=-YVCs_omWkLost_4) from the 2025 CVE Program Technical Workshop.

## Test CVE Services SADP Pilot Environment

Participating SADPs will have accounts and credentials for a test environment. The instance will use production data and sync roughly daily. SADPs will only be permitted to [add (PUT) SADP information](https://cveawg.mitre.org/api-docs/#/CVE%20Record/cveAdpUpdateSingle) to existing CVE Records and manage users for their SADP organizations.

We have not yet decided when (or even if) to shift from the test to production environments, but the plan is to shift the pilot to production.

## Required SADP Information

The point of SADP is for downstream Supplier CNAs to provide information about their Products with respect to an existing Vulnerability in an upstream Product. As such, the follwing information MUST be provided in an SADP container.

#### ADP Metadata
`containers.adp[].providerMetadata`

#### Product Status
`containers.adp[].affected`

## Optional SADP Information

An SADP MAY populate any other elements of an ADP container. One goal of the pilot is to determine if any elements cause confusion or conflict with other information in a Record, especially elements of the CNA container. CVSS, for example, [is designed to be reassessed in downstream uses](https://www.first.org/cvss/user-guide#Assessing-Vulnerabilities-in-Software-Libraries-and-Similar2:~:text=When%20assessing%20a%20vulnerability%20in%20a%20given%20implementation%20using%20the%20impacted%20library%2C%20the%20metric%20values%20must%20be%20re%2Dassessed%20for%20that%20specific%20implementation%2E), particularly for Products such as libraries or operating system kernels.

## Participants and Scope

CVE consumers should expect to see SADP containers provided by the pilot participants subject to their defined scopes.

| Participant | Scope | Examples |
| :--- | :--- | :--- |
| HeroDevs | .NET 6, others? | |
| Microsoft | chromium, others? | |
| Red Hat | lots of managed software packages | |
