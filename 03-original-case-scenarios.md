# Original Case Scenarios

The following scenarios explore how URL Filtering, Cloud App Control, and complementary ZIA security controls can address different application-access requirements.

All examples are fictional and independently developed for conceptual understanding.

---

## Case 01 — Design Collaboration Without Unrestricted Publishing

### Scenario

A creative team uses a hosted design platform to collaborate on visual projects.

Access to the platform is approved because employees need to review design boards, exchange feedback, and inspect shared assets.

However, the organisation does not want every user to publish projects externally or make internal designs publicly accessible.

### Security Requirement

The requirement is not to block the entire application.

Instead, the organisation wants to permit normal application usage while restricting specific publishing or sharing operations.

### Policy Consideration

Cloud App Control is the natural starting point, provided the application is recognised and the relevant publishing or sharing activity is supported.

URL Filtering can complement this policy where specific destinations associated with the service also require restrictions.

If an explicit application allow would otherwise prevent the URL rule from being evaluated, URL Cascading becomes relevant.

### Why URL Filtering Alone Is Insufficient

Blocking the entire destination would prevent legitimate design collaboration.

Allowing the destination would not inherently distinguish reviewing a design from publishing it externally.

The distinction is application activity, not simply website access.

### Design Considerations

- Confirm the application's supported activity controls.
- Verify that the relevant browser workflow is recognised.
- Consider whether publishing and sharing are separate supported actions.
- Identify whether additional destination restrictions are required.
- Validate cascading behaviour if both policy types must evaluate the same transaction.

---

## Case 02 — Personal Cloud Storage With Restricted Transfers

### Scenario

A research group needs to view reference material stored in an external cloud-storage service.

The organisation permits access to the service for legitimate research activities.

However, unrestricted file transfers introduce a data-handling concern.

### Security Requirement

The organisation wants to distinguish between accessing stored information and transferring files.

Two separate questions arise:

1. Is the user allowed to access the cloud-storage application?
2. Is the user allowed to upload or download files?

### Policy Consideration

Cloud App Control is relevant for restricting supported upload and download activities within the recognised application.

URL Filtering can separately restrict access to the service or associated destinations.

DLP becomes relevant if the concern extends to the sensitivity of the information being transferred.

### Why Not Simply Block the Storage Category?

A category-level block could prevent access to legitimate services along with the unwanted destinations.

The security requirement is more specific than simply prohibiting cloud storage.

### Additional Control: DLP

Suppose file transfers are permitted for ordinary research documents but prohibited when they contain protected information.

A blanket upload restriction may be unnecessarily restrictive.

DLP can inspect supported content against configured data-protection rules.

This creates a distinction between:

- Restricting the upload activity.
- Inspecting the uploaded content.

### Design Considerations

Cloud-storage services may use several hostnames, APIs, browser endpoints, and content delivery services.

Application recognition and traffic coverage must be validated for the relevant workflow.

---

## Case 03 — Streaming Access With Destination-Level Restrictions

### Scenario

An organisation permits a streaming platform for a particular user population.

However, some destinations associated with the service must remain inaccessible because they fall under a restricted content category.

### Security Requirement

The application itself is approved, but the organisation still requires destination-level restrictions.

### Policy Consideration

Cloud App Control can express the application-level permission where supported.

URL Filtering can enforce the destination/category restriction.

URL Cascading becomes relevant when the explicit Cloud App Control allow would otherwise prevent URL Filtering from evaluating the transaction.

### Why Cloud App Control Alone May Not Be Enough

Application-level access and destination-level classification are different requirements.

A permitted application does not necessarily imply that every associated destination should receive unrestricted access.

### Design Considerations

- Confirm the actual destination involved.
- Check whether ZIA recognises it as part of the intended application.
- Confirm the URL category assigned to the destination.
- Evaluate the impact of cascading.
- Consider custom URL categories if standard categorisation is too broad.

---

## Case 04 — Developer Collaboration and Source-Code Protection

### Scenario

An engineering group uses a hosted developer collaboration platform.

Employees require access to repositories, project discussions, and issue tracking.

However, the organisation wants to restrict certain content-transfer activities and reduce the risk of confidential source code or credentials being transferred to unapproved locations.

### Security Requirement

The platform is approved, but not every application operation or data-transfer destination should be unrestricted.

### Policy Consideration

Cloud App Control can govern supported activities within the recognised application.

DLP becomes relevant when the requirement concerns the actual content being transferred.

URL Filtering can enforce destination restrictions for unrelated or unapproved developer services.

### Why Multiple Controls?

Consider an upload containing a sensitive credential.

Cloud App Control may identify the upload operation.

However, knowing that an upload is occurring is not equivalent to knowing that its contents contain a private key or credential.

DLP introduces content-aware inspection where supported.

### Additional Considerations

- SSL/TLS inspection may be required for deeper content visibility.
- API and browser workflows may have different inspection coverage.
- Application recognition must be validated.
- File-transfer channels must be considered individually.
- DLP capabilities depend on supported protocols and inspection conditions.

---

## Case 05 — Malicious Domain Requiring DNS-Layer Enforcement

### Scenario

A security team identifies a newly registered domain associated with a malicious campaign.

The organisation wants clients using its protected DNS path to be prevented from resolving the domain.

### Security Requirement

The organisation wants to restrict name resolution before a conventional web session is established.

### Policy Consideration

DNS Control is the relevant policy surface for DNS requests that reach the configured Zscaler DNS enforcement path.

URL Filtering remains useful for web destination restrictions.

Firewall Control can provide an additional network-layer restriction where appropriate.

### Why URL Filtering Alone Is Not Equivalent

A DNS lookup can originate from processes other than a browser.

An application may attempt to resolve a hostname without opening a conventional web page.

DNS Control addresses the DNS-layer request.

URL Filtering primarily addresses web destination access.

### Design Considerations

DNS-layer enforcement is dependent on traffic reaching the intended resolver and policy path.

Potential coverage limitations include:

- Alternate DNS resolvers.
- Encrypted DNS.
- DNS caching.
- Direct IP connections.
- Hard-coded IP addresses.
- Traffic outside the configured forwarding path.

A DNS block should not be interpreted as proof that every possible access route has been blocked.

---

## Case 06 — Sensitive Information Transfer to an Approved Service

### Scenario

A professional-services team uses an approved external collaboration platform.

Employees can share ordinary documents, but confidential customer identifiers must not be transferred outside the organisation.

### Security Requirement

The organisation needs to distinguish between an approved application operation and an unacceptable data-transfer event.

### Policy Consideration

Cloud App Control can restrict the upload activity where the application supports that action.

DLP is relevant when the decision depends on the actual content.

For example, a document may be permitted when it contains ordinary project notes but blocked when it contains protected customer information.

### Why Not Only Cloud App Control?

Application-aware policy may identify that an upload is taking place.

It does not automatically determine whether the document contains sensitive information.

### Why Not Only DLP?

DLP is not a replacement for general application access control.

If all uploads to a particular service are prohibited, an application-level restriction may be more direct.

If only specific content is prohibited, DLP provides the more appropriate distinction.

### Design Considerations

- Define the sensitive data being protected.
- Confirm supported content inspection.
- Verify SSL/TLS inspection requirements.
- Consider file formats and transfer channels.
- Review DLP exceptions and policy scope.

---

## Case 07 — Unknown Cloud Application and Risk-Based Access

### Scenario

A user begins accessing an unfamiliar cloud service that has not been explicitly approved or classified by the organisation.

The organisation wants to avoid automatically granting unrestricted access to unknown services.

At the same time, blocking every unfamiliar destination may disrupt legitimate work.

### Security Requirement

The organisation needs a baseline destination policy and an appropriate approach to applications with unknown or elevated risk.

### Policy Consideration

URL Filtering can enforce baseline destination restrictions.

Where available, Cloud App Control risk information and application categories can contribute to application governance.

Browser Isolation may provide a contained browsing experience for supported destinations and use cases.

Threat Protection can provide additional security inspection.

### Why Not Automatically Block Everything?

An unfamiliar application is not necessarily malicious.

Its risk profile, business purpose, reputation, and actual traffic characteristics should be considered.

### Design Considerations

- Verify application classification.
- Review risk and sanctioned status where available.
- Determine whether the service is genuinely required.
- Consider whether isolated access is appropriate.
- Avoid treating application risk scores as infallible.
- Review policy decisions and logs.

---

## Scenario Comparison

| Scenario | Primary Policy | Complementary Control |
|---|---|---|
| Design collaboration | Cloud App Control | URL Filtering / Cascading |
| Personal cloud storage | Cloud App Control | DLP |
| Streaming destination restriction | URL Filtering + Cloud App Control | Cascading |
| Developer collaboration | Cloud App Control | DLP / SSL Inspection |
| Malicious domain | DNS Control | URL Filtering / Firewall |
| Sensitive information transfer | DLP | Cloud App Control |
| Unknown cloud application | URL Filtering / Risk-based App Control | Browser Isolation |

---

## Overall Observation

These scenarios demonstrate that a security requirement may involve more than one policy engine.

The correct approach depends on what is actually being restricted:

- A destination.
- An application.
- An application activity.
- A DNS request.
- A network connection.
- A file category.
- Sensitive information.
- A potentially malicious browsing session.

Selecting the right enforcement layer is more important than simply increasing the number of configured policies.

---
