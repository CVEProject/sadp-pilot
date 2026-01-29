# Supplier ADP (SADP) Pilot

## Summary

This repository documents the Supplier-as-ADP Pilot project. This README and other documentation should be considered works-in-progress and are likely to be incomplete.

## Status and Schedule

Subject to change.

Test environment available in early February 2026.

SADP in production sometime in March 2026.

"Formally" review the pilot in July 2026. This period would catch April and July quarterly update cycles and four monthly update cycles.

This formal review should support a Program decision whether and how to continue SADP.

## Background

Software is made of other software. When the upstream software has a vulnerability, it'd be nice to know if and to what extent downstream software is affected, and what to do about it. This SADP pilot is a way for the CVE Program to decide if and how to support status information about downstream inheritance of upstream vulnerabilities. While not strictly planning to implement [Vulnerability Exploitability eXchange (VEX)](https://github.com/CVEProject/sadp-pilot/blob/main/README.md), the SADP pilot will essentially implement VEX, or at least meet the material [requirements](https://github.com/SBOM-Community/documents/blob/main/README.md#minimum-requirements-for-vex) of VEX.

One expected use case is that vulnerability scanning and management could be improved by combining SADP (VEX) information with existing detection mechanisms, reducing false positive and possibly other incorrect results (see [Q4](#q4)).

## Test CVE Services SADP Pilot Environment

Participating SADPs will have accounts and credentials for a test environment. The instance will use production data and sync roughly daily. SADPs will only be permitted to [add (PUT) SADP information](https://cveawg.mitre.org/api-docs/#/CVE%20Record/cveAdpUpdateSingle) to existing CVE Records and manage users for their SADP organizations.

We have not yet decided when (or even if) to shift from the test to production environments, but the plan is to shift the pilot to production. Before providing production SADP information, participants MUST demonstrate their capability in the test environment.

## Required SADP Information

The point of SADP is for downstream Supplier CNAs to provide information about their Products with respect to an existing Vulnerability in an upstream Product. As such, the follwing information MUST be provided in an SADP container. Consumers must be able to tell that an ADP container is an *S*ADP container, which means that the SADP information is related the SADP's use of (dependency on) the affected Products in the CNA container.

If an ADP container type (and/or `shortName`) indicates Supplier, then the Products in the ADP container use or depend on the Products in the CNA container and the status information is scoped to whether and to what extent the downstream (SADP) Products are affected by the upstream (CNA) Products.

#### ADP Metadata

`containers.adp[].providerMetadata`

`containers.adp[].providerMetadata.x_adpType` with the value: `supplier`.  This would require a future CVE Record Format change and would support other types of ADP such as `enricher`. Another option is to overload `.containers.adp.providerMetadata.shortName` with a string like `"${shortName}-SADP"`.
#### Product Status

`containers.adp[].affected`

#### Product Status Reference

As an alternative to providing product status within the their container, an SADP MAY instead provide a reference to external product status. The format of this reference has not been determined yet. One simple option is a reference URL with a new type (add example). It may be better, or even necessary, to develop a more robust reference schema, more aligned with this [RFD about assertions](https://github.com/CVEProject/cve-schema/pull/472) (add example).

## Optional SADP Information

An SADP MAY populate any other elements of an ADP container. One goal of the pilot is to determine if any elements cause confusion or conflict with other information in a Record, especially elements of the CNA container. CVSS, for example, [is designed to be reassessed in downstream uses](https://www.first.org/cvss/user-guide#Assessing-Vulnerabilities-in-Software-Libraries-and-Similar2:~:text=When%20assessing%20a%20vulnerability%20in%20a%20given%20implementation%20using%20the%20impacted%20library%2C%20the%20metric%20values%20must%20be%20re%2Dassessed%20for%20that%20specific%20implementation%2E), particularly for Products such as libraries and operating system kernels.

## JSON Reference

Here ([CVE-2025-14174](https://github.com/CVEProject/sadp-pilot/blob/main/CVE-2025-14174_sadp.json)) is an manually generated example CVE Record with an SADP container. Look for `x_` field names.

## Participants and Scope

CVE consumers should expect to see SADP containers provided by these Supplier participants subject to their defined scopes.

| Participant | Scope | Examples |
| :--- | :--- | :--- |
| HeroDevs | .NET 6, others? | |
| Microsoft | chromium, others? | |
| Red Hat | lots of managed software packages | |
| Oracle | ? | |

In addition to these primary SADP content producers (downstream Suppliers), we have discussed SADP with upstream Suppliers and institutional CVE consumers, specifically vulnerability scanners (see [Q4](#q4)). We should also talk to vulnerability scanner users. We may solicit active participation from these types of organizations to help determine if SADP is useful (or harmful) and if changes are needed.

## Questions

We both know some questions in advance, and we expect new questions and experience to arise from running the pilot.

Q1: Can I be and ADP? How do I become an ADP?

A1: This pilot is limited in scope to Supplier ADPs. If you are a Supplier CNA and wish to participate in the SADP pilot, we may be able to add you during the pilot period. The Program plans to consider and test other types of ADPs, for example, "enrichment" ADPs such as [CISA Vulnrichment](https://github.com/cisagov/vulnrichment) and the CVE Program [references ADP](https://github.com/CVEProject/cvelistV5/blob/main/README.md#cve-program-container). There is currently no general purpose or Program-wide policy or criteria about ADPs.

Q2. There really is a lot of software reuse and dependency. How will CVE manage all that information, technically and procedurally?

Q3. The pilot was a failure, but we added content to the production CVE corpus. How do we roll back?

[Q4](#q4). A big assumption behind VEX and SADP is that consumers of (CVE) dependency-related vulnerability information can and will use that information to improve vulnerability scanning results. For example, in downstream software, a scanner detects a version of an upstream library with a known vulnerability. Lacking further information, the scanner returns a positive (vulnerable) result. But if the scanner consumes SADP information that conveys that the downstream use of the library is not vulnerable (or not exploitable), then the scanner returns a negative (not vulnerable) result. Signal to noise ratio improved! So, is this assumption valid? What do vulnerability scanners think? What about other defenders who scan, either rolling their own or as users of scanning products?

Q5. How do we measure the results (success, failure)?

Q6. Should the CVE Program shift the SADP pilot into full production? How does the Program make this decision?

Q7. How long will the Pilot run?

A7. Estimates: Test period in February and March 2026, production for ~four months, then a formal review leading to a decision whether and how to continue.

Q8. How do I know if a CVE Record has SADP information?

A8. First, check the Record for an ADP container of [`"x_adpType": "supplier"`](https://github.com/CVEProject/sadp-pilot/blob/740d3bd121dbd1b92ca3c16eb65dd749bb62914b/CVE-2025-14174_sadp.json#L61) (and/or [`shortName` ends with `-SADP`](https://github.com/CVEProject/sadp-pilot/blob/740d3bd121dbd1b92ca3c16eb65dd749bb62914b/CVE-2025-14174_sadp.json#L125)). Second, we plan to provide a running list of CVE IDs that have SADP containers.

## References

Two very work-in-progress documents:

* [Supplier-CNA-as-ADP Pilot Overview](https://docs.google.com/document/d/1JLF5vj_8W7KVeqJNhQl8Fj7PLv-ujbqMLTU6LtDlmvI)
* [Supplier-CNA-as-ADP Pilot](https://docs.google.com/document/d/1u2qKAYo7AYOCDYVv1Le58kv5YpzHBsrwuib7OPY-YyU)

[Slides](https://youtu.be/dBtlUz0kUww?si=-YVCs_omWkLost_4) from the 2025 CVE Program Technical Workshop.
