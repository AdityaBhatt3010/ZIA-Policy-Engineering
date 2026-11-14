# Foundations and Policy Selection

## 1. Understanding the Problem

A web transaction is not defined solely by its destination URL.

When a user accesses a cloud application, multiple dimensions become relevant:

- Destination hostname and URL.
- Application identity.
- Requested application activity.
- User and group identity.
- Network location and device context.
- Protocol and connection characteristics.
- Transferred file type.
- Content being transferred.
- Threat and security characteristics.

Different ZIA policy engines address different dimensions of this transaction.

Understanding these differences is essential when deciding which policy should enforce a particular requirement.

---

## 2. URL Filtering

URL Filtering is primarily concerned with controlling access to web destinations.

It evaluates requests using URL categories and other supported destination criteria, together with applicable policy conditions such as users, groups, locations, and time.

### Primary Capabilities

- Allow or block website categories.
- Restrict access to defined destinations.
- Apply different access policies to different user populations.
- Use custom URL categories.
- Enforce destination-based access restrictions.

### Conceptual Example

An organisation permits access to educational resources but restricts access to gambling-related websites.

The security requirement is destination-oriented. The organisation is not concerned with an activity inside a particular cloud application.

URL Filtering is therefore the natural policy surface.

### Important Limitation

Allowing a destination does not automatically mean that every activity inside the application is permitted.

A user may be allowed to access a cloud service while still requiring restrictions on uploading, downloading, publishing, or sharing.

URL Filtering alone should not be treated as a universal application-activity enforcement mechanism.

---

## 3. Cloud App Control

Cloud App Control provides application-aware policy enforcement for supported cloud applications.

Instead of considering only the destination, it recognises an application and exposes supported application-level access or activity controls.

Depending on the application, available actions may include:

- Application access.
- Chat or messaging.
- File uploads.
- File downloads.
- Publishing.
- Sharing.
- Other application-specific activities.

The exact activity options depend on the application integration and current Zscaler support.

### Conceptual Example

A user is permitted to access a cloud-storage service to view existing documents.

However, transferring new files to the service is restricted.

The requirement is no longer simply about reaching a destination.

It concerns an operation performed within a recognised application.

Cloud App Control is therefore a more appropriate policy surface, provided the relevant application and activity are supported.

### Important Limitation

Application recognition does not guarantee that every client or communication path is covered.

A browser session, native desktop application, mobile application, and API integration may behave differently.

---

## 4. The Fundamental Difference

| Parameter | URL Filtering | Cloud App Control |
|---|---|---|
| Primary focus | Destination | Application |
| Policy identification | URL/category | Recognised application |
| Activity awareness | Not its primary purpose | Supported application activities |
| Typical objective | Website access control | Application usage control |
| Granularity | Destination/category-oriented | Application/activity-oriented |
| Example | Block a website category | Allow an app but restrict uploads |
| Complementary use | Destination restrictions | Application-specific restrictions |

### Mental Model

Consider a building with controlled entry.

- URL Filtering determines whether someone can enter the building.
- Cloud App Control determines which activities they can perform once inside.
- DLP evaluates whether sensitive information is being transferred.
- File Type Control governs specific file categories.
- DNS Control can prevent a domain lookup.
- Firewall Control governs network connections.

These controls may contribute to the same security objective, but their enforcement responsibilities differ.

---

## 5. Why Both Policies May Be Required

Consider an organisation that permits a particular cloud application.

The application itself is approved, but some destinations associated with its service must remain restricted.

Two separate requirements exist:

1. Application access must be permitted.
2. Certain destinations must still be evaluated against URL policy.

Cloud App Control can address the first requirement.

URL Filtering can address the second.

However, policy precedence becomes important.

An explicit Cloud App Control allow normally prevents URL Filtering from evaluating the transaction unless URL Cascading is enabled.

This is why the relationship between these policies matters as much as their individual configurations.

---

## 6. Selecting the Appropriate Policy

| Security Requirement | Policy to Consider |
|---|---|
| Block a website category | URL Filtering |
| Allow a destination group | URL Filtering |
| Restrict a recognised cloud application | Cloud App Control |
| Restrict uploads within a supported application | Cloud App Control |
| Restrict a destination after an application allow | URL Cascading |
| Block DNS requests to a domain | DNS Control |
| Restrict a network connection | Firewall Control |
| Prevent specific file types | File Type Control |
| Detect sensitive information | DLP |
| Inspect eligible HTTPS traffic | SSL/TLS Inspection |
| Contain risky browsing | Browser Isolation |

---

## 7. Policy Design Considerations

The selection of a policy should begin with the actual security requirement.

Consider the following:

- Is the requirement about a destination or an application?
- Is a particular activity being restricted?
- Does the application expose the required activity control?
- Is the traffic identified correctly?
- Does another policy engine evaluate the transaction?
- Is URL Cascading required?
- Does the policy depend on SSL/TLS inspection?
- Is the relevant traffic actually traversing ZIA?

Using a policy simply because it can produce a block is not necessarily the correct design.

The policy should represent the security objective as accurately as possible.

---
