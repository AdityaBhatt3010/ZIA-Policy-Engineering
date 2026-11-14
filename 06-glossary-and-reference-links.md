# Glossary and Reference Links

## 1. Glossary

| Term | Explanation |
|---|---|
| ZIA | Zscaler Internet Access |
| URL Filtering | Destination and URL-category-based access policy |
| Cloud App Control | Application-aware access and activity policy |
| URL Category | Classification assigned to web destinations |
| Custom URL Category | Administrator-defined collection of destinations |
| Cloud Application | Application recognised by Zscaler |
| Application Activity | An operation performed within a supported application |
| URL Cascading | Applying URL Filtering after an explicit Cloud App Control allow |
| DNS Control | Policy enforcement for DNS requests |
| Firewall Control | Network traffic enforcement |
| File Type Control | File-category-based transfer restrictions |
| DLP | Data Loss Prevention |
| SSL/TLS Inspection | Inspection or bypass policy for eligible encrypted traffic |
| Sandbox | Environment for analysing supported suspicious files |
| Browser Isolation | Contained browsing for supported scenarios |
| Rule Precedence | Order in which relevant policy rules are evaluated |
| Do Not Inspect | SSL/TLS policy action excluding eligible traffic from decryption |
| Application Recognition | Identification of an application within inspected traffic |

---

## 2. Official Zscaler References

The following documentation provides additional technical context.

### URL Filtering

- [About URL Filtering](https://help.zscaler.com/legacy-zia/about-url-filtering)
- [Recommended URL Filtering Policy](https://help.zscaler.com/zia/recommended-url-filtering-policy)

### Cloud App Control

- [About Cloud App Control](https://help.zscaler.com/zia/about-cloud-app-control)
- [Adding a System & Development Rule for Cloud App Control](https://help.zscaler.com/zia/adding-system-development-rule-cloud-app-control)

### Policy Cascading and Advanced Settings

- [Configuring Advanced Settings](https://help.zscaler.com/zia/configuring-advanced-settings)
- [ZIA Policy Leading Practices Guide](https://help.zscaler.com/zscaler-deployments-operations/zia-policy-leading-practices-guide)

### SSL/TLS Inspection

- [Configuring SSL/TLS Inspection Policy](https://help.zscaler.com/zia/configuring-ssl-inspection-policy)
- [Deployment Scenarios for SSL/TLS Inspection](https://help.zscaler.com/zia/deployment-scenarios-ssl-inspection)
- [Deploying SSL/TLS Inspection](https://help.zscaler.com/zia/how-do-i-deploy-ssl-inspection)

---

## 3. Documentation Notes

- Prefer current Zscaler Help Center documentation when older and newer articles describe the same feature.
- Some legacy documentation uses older navigation names and terminology.
- Available Cloud App Control activities depend on the application and product support.
- Cascading behaviour and predefined-rule exceptions should be checked against current documentation.
- Feature availability may depend on licensing and tenant configuration.

---

**Author:** Aditya Bhatt

*Independent technical reference for Zscaler Internet Access policy behaviour and layered web security.*

---
