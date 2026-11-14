# Policy Order and URL Cascading

## 1. Understanding Policy Precedence

URL Filtering and Cloud App Control can influence the outcome of the same web transaction.

However, they are not evaluated as two independent policies whose results are always combined.

In the documented ZIA policy interaction, Cloud App Control is evaluated before URL Filtering.

This creates an important distinction between an explicit application decision and the absence of an application match.

## 2. Default Policy Evaluation

The conceptual flow is:

```text
              Web Transaction
                     |
                     v
           Cloud App Control
               Evaluation
                     |
          +----------+----------+
          |          |          |
          v          v          v
        BLOCK      ALLOW     NO MATCH
          |          |          |
          v          v          v
       BLOCKED   URL Filtering
                 Normally       URL Filtering
                 Skipped        Evaluation
                    |               |
                    v               v
               APP ALLOW       URL POLICY
                               DECISION
```

The key point is that an explicit Cloud App Control allow normally short-circuits URL Filtering evaluation.

When Cloud App Control has no matching rule, URL Filtering can still evaluate the request.

An explicit application block remains a block.

---

## 3. What Is URL Cascading?

URL Cascading allows URL Filtering to evaluate a transaction after Cloud App Control has explicitly allowed it.

Under the default behaviour, an application allow normally ends the relevant policy evaluation path before URL Filtering.

With cascading enabled, the allowed transaction proceeds to URL Filtering for an additional destination-based decision.

### Without Cascading

Consider a recognised application with the following policy conditions:

**Cloud App Control**
- Application: Design Platform
- Action: Allow

**URL Filtering**
- Destination category: Restricted
- Action: Block

If the transaction matches the explicit Cloud App Control allow and cascading is disabled, URL Filtering normally does not evaluate that transaction.

The application allow governs the outcome.

### With Cascading

The same transaction is allowed by Cloud App Control initially.

However, cascading permits URL Filtering to evaluate it afterwards.

If the destination matches the restricted URL category, URL Filtering can apply its block action.

The final outcome is therefore different.

This is the principal purpose of URL Cascading.

---

## 4. Policy Behaviour Matrix

| Cloud App Control | Cascading | URL Filtering | Outcome |
|---|---|---|---|
| Explicit Block | Disabled | Not used to override | Block |
| Explicit Block | Enabled | Cannot override the block | Block |
| Explicit Allow | Disabled | Normally skipped | Allow |
| Explicit Allow | Enabled | Evaluated | URL policy determines subsequent outcome |
| No Match | Disabled | Evaluated | URL policy determines outcome |
| No Match | Enabled | Evaluated | URL policy determines outcome |

This matrix describes the simplified policy interaction and should not be interpreted as a complete representation of every ZIA policy engine or exception.

---

## 5. Understanding the Allow Cascade Setting

ZIA provides cascading configuration through advanced settings and, in supported rule experiences, rule-level configuration.

Two important concepts are involved.

### A. Global Cascading

The global setting allows Cloud App Control permits to cascade into URL Filtering more broadly.

This can have a significant effect on existing policy behaviour.

For example, an application that was previously allowed without further URL evaluation may now encounter URL Filtering restrictions.

This can introduce unintended blocks if broad URL categories or custom URL lists were not designed with application cascading in mind.

### B. Rule-Level Cascading

Where supported, a Cloud App Control rule can expose a setting such as:

`Cascade to URL Filtering`

This allows cascading behaviour to be associated with an individual rule.

The distinction is important:

- Global cascading affects a wider policy scope.
- Rule-level cascading offers more targeted control.
- Available options depend on the current product configuration and UI.
- Global settings may take precedence over or affect rule-level behaviour.

Always validate the actual setting interaction in the current tenant.

---

## 6. Why Cascading Is Not Always Recommended

Enabling cascading everywhere may sound like an easy way to ensure both policies are enforced.

However, this can create unintended policy interactions.

Consider an organisation that has:

- Broad URL category restrictions.
- Application-specific allows.
- Custom URL categories.
- Different user groups.
- Several overlapping application rules.

Enabling global cascading may cause previously permitted application transactions to encounter URL Filtering rules that were not originally intended to govern them.

Potential consequences include:

- Unexpected application restrictions.
- Conflicting policy expectations.
- Additional troubleshooting complexity.
- Unintended blocks caused by broad URL categories.
- Difficulties identifying which policy is responsible for an outcome.

Zscaler's policy-leading-practices guidance should be consulted before adopting a global cascading approach.

---

## 7. Application-Specific Exceptions

Not every predefined application rule necessarily follows the same general behaviour.

Zscaler documents exceptions involving certain Microsoft 365 One Click rules.

In such cases, a higher-priority custom Cloud App Control rule may be required to achieve the intended cascading behaviour.

The broader lesson is that policy precedence and predefined rules must be considered together.

Do not assume that every application behaves identically simply because the same policy type is being used.

---

## 8. Conceptual Example: Two Different Security Requirements

A fictional collaboration platform is permitted for a group of users.

However, a particular destination associated with the service must remain restricted.

The organisation has two separate objectives:

**Application objective**

The recognised collaboration platform should be available.

**Destination objective**

A restricted destination should remain inaccessible even if the application itself is permitted.

Cloud App Control addresses the first requirement.

URL Filtering addresses the second.

Cascading connects the two policy decisions.

This is not redundant policy configuration. Each policy is enforcing a different condition.

---

## 9. Understanding the Evaluation Difference

A useful way to interpret the interaction is:

```text
Cloud App Control ALLOW
          |
          +---- Cascading OFF
          |          |
          |          v
          |      Application allowed
          |
          +---- Cascading ON
                     |
                     v
               URL Filtering
                     |
              +------+------+
              |             |
              v             v
            ALLOW          BLOCK
              |             |
              v             v
          APP ALLOWED    ACCESS BLOCKED
```

The important distinction is that cascading introduces an additional destination-policy decision after an application allow.

It does not provide a mechanism for URL Filtering to override an explicit Cloud App Control block.

---

## 10. Key Takeaways

1. Cloud App Control is evaluated before URL Filtering in the documented interaction.
2. An explicit Cloud App Control block remains blocked.
3. An explicit Cloud App Control allow normally prevents URL Filtering evaluation by default.
4. Cascading allows URL Filtering to evaluate an application transaction after an explicit allow.
5. No matching Cloud App Control rule does not mean the transaction automatically bypasses URL Filtering.
6. Global cascading and rule-level cascading have different scopes.
7. Application-specific exceptions must be considered.
8. Cascading should be introduced based on an identified security requirement rather than enabled indiscriminately.

**The central idea:** Cloud App Control and URL Filtering can work together, but understanding their evaluation order is essential to predicting the actual enforcement outcome.

---
