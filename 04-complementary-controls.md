# Complementary ZIA Security Controls

## 1. Introduction

URL Filtering and Cloud App Control are important components of web access enforcement.

However, they do not cover every possible security requirement.

A request involving a malicious domain, an unauthorised network connection, a sensitive file upload, or an encrypted session may require a different enforcement mechanism.

ZIA provides additional security controls that can complement application-aware and destination-based policies.

These controls should be understood according to their enforcement objectives rather than treated as interchangeable alternatives.

---

## 2. DNS Control

### What Is DNS Control?

DNS Control provides policy enforcement for DNS requests that reach the configured Zscaler DNS enforcement path.

DNS translates human-readable domain names into IP addresses.

Before a client establishes a conventional web session with a hostname, it may need to resolve that hostname through DNS.

DNS Control introduces the ability to govern this resolution process.

### Security Applications

- Blocking DNS requests to malicious domains.
- Restricting access to selected domain categories.
- Applying DNS-layer restrictions.
- Supporting an additional layer of domain protection.

### How It Differs From URL Filtering

URL Filtering evaluates web destinations.

DNS Control evaluates DNS requests.

For example:

A browser requests access to a domain.

The DNS resolution process may be restricted before the browser establishes the intended web connection.

This is a different enforcement point from evaluating the resulting web request.

### Limitations

DNS Control does not automatically prevent every possible method of reaching a destination.

Coverage can be affected by:

- Alternative DNS resolvers.
- Encrypted DNS.
- Cached DNS responses.
- Direct IP connections.
- Hard-coded IP addresses.
- Traffic that does not traverse the configured DNS enforcement path.

**Key takeaway:** DNS Control governs name-resolution requests, while URL Filtering governs web destination access.

---

## 3. Firewall Control

### What Is Firewall Control?

Firewall Control governs network traffic based on supported network-level criteria.

These may include:

- Source and destination IP addresses.
- Ports.
- Protocols.
- Other supported network attributes.

### Security Applications

- Restricting outbound connections.
- Controlling access to specific network services.
- Blocking unwanted ports or protocols.
- Applying network-level access restrictions.

### How It Differs From Cloud App Control

Firewall Control is concerned with the network connection.

Cloud App Control is concerned with recognised cloud applications and supported application activities.

Consider a user connecting to a permitted network service.

A firewall decision may allow the connection.

That does not automatically mean every operation performed through the application should be permitted.

Application-level controls address a different layer of the security requirement.

**Key takeaway:** Firewall Control governs network flows, not every application operation.

---

## 4. File Type Control

### What Is File Type Control?

File Type Control provides restrictions based on supported file-type classification.

It can be used to govern the transfer of selected file categories.

### Security Applications

- Restricting executable file transfers.
- Blocking selected archive types.
- Preventing specified file categories from being downloaded or uploaded.
- Applying file-oriented restrictions across supported traffic.

### Difference Between File Type Control and DLP

These two controls answer different questions.

**File Type Control:**

What type of file is being transferred?

**DLP:**

Does the content contain information that violates a configured data-protection policy?

For example:

A PDF document may be an allowed file type.

However, it may still contain confidential customer information.

File Type Control and DLP therefore complement one another.

**Key takeaway:** File type classification is not equivalent to content inspection.

---

## 5. Data Loss Prevention (DLP)

### What Is DLP?

Data Loss Prevention focuses on identifying and controlling the transfer of sensitive information.

It evaluates content against configured data-protection policies, subject to supported protocols and inspection capabilities.

### Security Applications

- Preventing sensitive information from leaving the organisation.
- Identifying protected identifiers.
- Detecting confidential information in supported uploads.
- Enforcing data-handling requirements.
- Applying content-aware restrictions.

### Difference Between DLP and Cloud App Control

Cloud App Control can determine whether a supported application activity is permitted.

DLP evaluates whether the content itself violates a data-protection requirement.

Consider an application that allows file uploads.

An organisation may permit ordinary project documents but prohibit documents containing confidential customer information.

Cloud App Control can govern the upload operation.

DLP can evaluate the uploaded content.

### Important Considerations

DLP inspection depends on:

- Supported protocols.
- Content visibility.
- SSL/TLS inspection where applicable.
- File format support.
- Configured identifiers and policies.
- Applicable exclusions.

**Key takeaway:** Cloud App Control can govern the action; DLP can govern the information being transferred.

---

## 6. SSL/TLS Inspection

### What Is SSL/TLS Inspection?

Most modern web traffic uses encrypted connections.

SSL/TLS Inspection determines how eligible encrypted traffic is handled for inspection.

Depending on policy, traffic may be inspected or excluded from decryption.

### Why Is It Important?

Encrypted traffic limits visibility into the contents of a communication.

Some security controls require deeper inspection to evaluate:

- Uploaded content.
- Downloaded files.
- Malware characteristics.
- Sensitive information.
- Other supported content-level signals.

SSL/TLS Inspection can therefore provide visibility required by other policy engines.

### Important Distinctions

Enabling SSL/TLS Inspection does not automatically mean every HTTPS connection is decrypted.

Several factors affect inspection:

- Inspection policy.
- Do Not Inspect rules.
- Certificate trust.
- Application compatibility.
- Certificate pinning.
- Traffic exclusions.
- Supported protocols.

### Do Not Inspect Behaviour

A Do Not Inspect decision should not automatically be interpreted as an instruction to bypass every other security policy.

The configured policy action and its associated settings determine whether other policy evaluation continues or is bypassed.

This distinction matters when troubleshooting unexpected results.

**Key takeaway:** SSL/TLS Inspection is primarily a visibility and inspection decision, not a universal website allow/block policy.

---

## 7. Threat Protection and Sandbox

### Threat Protection

Threat Protection provides security inspection against malicious or suspicious content, subject to the enabled services and traffic visibility.

Relevant use cases include:

- Malware detection.
- Malicious downloads.
- Threat-related URL enforcement.
- Suspicious content detection.

### Sandbox

Sandboxing provides an analysis environment for supported suspicious files or content.

It can help identify malicious behaviour that may not be apparent from the filename or file extension alone.

### Relationship With Other Policies

A website can be permitted by URL Filtering while the content downloaded from it is identified as malicious.

Similarly, a cloud application can be approved while a particular file transfer is still subject to threat inspection.

Access permission does not automatically establish content safety.

---

## 8. Browser Isolation

### What Is Browser Isolation?

Browser Isolation provides a contained browsing experience for supported destinations and scenarios.

Rather than treating every risky destination as either completely permitted or completely blocked, an organisation may choose to isolate the browsing session.

### Security Applications

- Containing browsing to unfamiliar destinations.
- Reducing direct endpoint exposure.
- Providing controlled access to selected risky websites.
- Supporting a middle ground between unrestricted access and blocking.

### Relationship With URL Filtering

URL Filtering can determine whether a destination should be allowed, blocked, or handled using a supported alternative action.

Browser Isolation can provide the contained browsing experience where supported.

The two controls can therefore work together.

---

## 9. Cloud Application Risk Profiles

Cloud application risk information can support broader application governance.

Depending on the available features and licensing, application risk characteristics and sanctioned status can be used to inform policy decisions.

This can be useful when managing applications that have not been explicitly approved.

However, application risk information should not be interpreted as a complete understanding of every application workflow or data-transfer operation.

Risk classification is an input to policy design, not a replacement for it.

---

## 10. Putting the Controls Together

Consider a user attempting to upload a confidential file to an external cloud service.

The transaction can involve several different security questions.

```text
                  DNS Request
                       |
                       v
                  DNS Control
                       |
                       v
                Network Connection
                       |
                       v
                 Firewall Control
                       |
                       v
              Encrypted Web Session
                       |
                       v
                SSL/TLS Inspection
                       |
                       v
             Application Identification
                       |
                       v
                Cloud App Control
                       |
                       v
                URL Filtering
                 (if applicable)
                       |
                       v
                  File Transfer
                       |
                       v
                 File Type Control
                       |
                       v
                Content Evaluation
                       |
                       v
                       DLP
                       |
                       v
              Threat Protection /
                   Sandbox
```

This is a conceptual relationship diagram, not a guaranteed universal execution sequence.

Actual policy order, feature availability, and inspection behaviour depend on the configured ZIA traffic path and individual policy engines.

---

## 11. Policy Selection Matrix

| Requirement | Relevant Control |
|---|---|
| Restrict website categories | URL Filtering |
| Restrict supported application activities | Cloud App Control |
| Evaluate destinations after an app allow | URL Cascading |
| Block DNS requests | DNS Control |
| Restrict IP/port/protocol traffic | Firewall Control |
| Restrict file categories | File Type Control |
| Identify sensitive content | DLP |
| Inspect eligible encrypted traffic | SSL/TLS Inspection |
| Detect malicious files | Threat Protection / Sandbox |
| Contain risky browsing | Browser Isolation |
| Govern risky cloud applications | Cloud App Control / Risk Profiles |

---

## 12. Final Observation

The presence of multiple policy engines does not necessarily indicate overlapping or redundant security controls.

Each engine addresses a different dimension of a transaction.

The objective is to establish which security requirement must be enforced and select the policy surface capable of expressing it.

A well-designed ZIA policy architecture is not defined by how many policies are enabled.

It is defined by how accurately those policies enforce the intended security outcomes.

---
