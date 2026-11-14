# Design Considerations and Validation

## 1. Identity and Policy Scope

ZIA policy decisions can depend on more than the destination or application.

Relevant conditions may include:

- User identity.
- User groups.
- Locations.
- Location groups.
- Device context.
- Time-based conditions.
- Other supported policy criteria.

A correctly configured rule may appear ineffective if the request is attributed to a different user, originates from an unexpected location, or follows a different traffic-forwarding path.

Identity and policy scope should therefore be considered when analysing unexpected results.

---

## 2. Rule Order and Specificity

Policy precedence is an important part of ZIA enforcement.

A broad rule placed before a specific exception can produce outcomes that appear inconsistent with the administrator's intention.

A useful design principle is to establish clear policy intent:

- Specific application permissions.
- Specific activity restrictions.
- Defined destination exceptions.
- Broader baseline restrictions.
- Deliberate default behaviour.

However, rule ordering should be evaluated separately for each policy engine.

Do not assume that every ZIA policy type follows an identical evaluation model.

---

## 3. SSL/TLS Visibility

HTTPS traffic is encrypted by default.

The ability of a security policy to inspect traffic depends on whether the relevant session is eligible for inspection.

Important considerations include:

- Whether SSL/TLS Inspection applies.
- Whether the destination is excluded.
- Whether a Do Not Inspect rule matches.
- Whether certificate trust is correctly established.
- Whether the application supports inspection.
- Whether certificate pinning affects compatibility.
- Whether the relevant policy engine can access the required transaction information.

Application identification and content inspection are not the same thing.

An application may be identifiable without every part of its encrypted payload being available for inspection.

---

## 4. Traffic Steering and Coverage

A configured policy only governs traffic that reaches the relevant enforcement point.

Traffic steering and forwarding configuration are therefore important.

Potential factors include:

- Zscaler Client Connector.
- Forwarding configuration.
- Location configuration.
- DNS steering.
- Split tunnelling.
- Direct internet access.
- Traffic exclusions.
- Alternative DNS resolution.
- Unmanaged endpoints.

A policy should never be assumed to provide universal coverage without validating the relevant traffic path.

---

## 5. Application Recognition

Cloud App Control depends on application identification and supported activity recognition.

A product name appearing in the application catalogue does not necessarily establish complete coverage of every way that application can be used.

For example:

- Browser access.
- Desktop client access.
- Mobile application access.
- API integrations.
- Embedded application components.

These may involve different endpoints, protocols, and traffic patterns.

The relevant workflow should be validated rather than assuming identical behaviour across every client.

---

## 6. Cascading Design Considerations

Before enabling URL Cascading, consider:

- Which Cloud App Control allows require an additional URL decision?
- Which URL categories will apply after cascading?
- Are broad URL restrictions likely to affect approved applications?
- Are custom URL categories required?
- Are predefined application rules subject to exceptions?
- Is global cascading necessary?
- Can a narrower rule-level option express the requirement?
- Have both the allow and deny outcomes been validated?

Global cascading can have a wider effect than an individual rule setting.

Its impact should be understood before changing an existing policy environment.

---

## 7. Validation Matrix

The following questions provide a conceptual framework for validating policy behaviour.

| Area | Validation Question |
|---|---|
| Identity | Is the expected user or group recognised? |
| Application | Is the transaction identified as the intended application? |
| Activity | Is the attempted operation supported? |
| Destination | Does the expected URL category match? |
| Cascading | Does URL Filtering evaluate after the application allow? |
| Block precedence | Does an explicit app-level block remain enforced? |
| TLS | Is traffic inspected or excluded? |
| DNS | Does the request traverse the intended DNS path? |
| File | Is the file type or content visible to the relevant control? |
| Logging | Can the matched rule and resulting action be identified? |

---

## 8. Troubleshooting Unexpected Results

When an observed result differs from the expected policy outcome, collect the relevant evidence before changing policy.

Useful information includes:

1. Timestamp of the request.
2. User identity.
3. Source location.
4. Device context.
5. Exact hostname or URL.
6. Recognised application.
7. Activity attempted.
8. Cloud App Control matched rule.
9. Cloud App Control action.
10. URL Filtering matched rule.
11. Cascading configuration.
12. SSL/TLS inspection decision.
13. DNS or Firewall Control decision, if applicable.
14. Relevant policy logs.

The central troubleshooting question is:

**Which policy engine evaluated this transaction, which rule matched, and why?**

---

## 9. Common Misconceptions

### Misconception 1: URL Filtering and Cloud App Control are identical.

**Correction:** They can influence the same access outcome but address different policy dimensions.

URL Filtering is primarily destination-oriented. Cloud App Control is application-aware.

### Misconception 2: URL Filtering always runs after Cloud App Control.

**Correction:** An explicit Cloud App Control allow normally prevents URL Filtering evaluation unless cascading is enabled.

### Misconception 3: URL Cascading allows URL Filtering to override a Cloud App Control block.

**Correction:** Cascading applies to the application allow path. It does not reverse an explicit application block.

### Misconception 4: Every cloud application supports the same activities.

**Correction:** Available activities depend on application recognition and current product support.

### Misconception 5: DNS Control blocks every possible route to a domain.

**Correction:** DNS Control governs DNS requests reaching its configured enforcement path. Alternative resolution and direct IP access are separate considerations.

### Misconception 6: SSL/TLS Inspection decrypts every HTTPS session.

**Correction:** Inspection policy, exclusions, certificate trust, application compatibility, and other conditions affect coverage.

### Misconception 7: File Type Control and DLP are interchangeable.

**Correction:** File type classification and content-sensitive inspection are separate security capabilities.

### Misconception 8: An allowed application is automatically safe.

**Correction:** Application permission does not establish that its content, downloads, uploads, or every destination are safe.

---

## 10. Final Design Principle

Policy selection should follow the security requirement.

A destination restriction, application activity restriction, DNS restriction, network restriction, and sensitive-data restriction are different problems.

The objective is not to apply every available control to every transaction.

It is to select the relevant controls, understand their interaction, and validate that the resulting behaviour matches the intended security outcome.

---
