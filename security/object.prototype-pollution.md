---
description: Prototype Pollution is a vulnerability affecting JavaScript.
---

# Object.prototype pollution

[Affecting **lodash.merge** package, **ALL** versions](https://snyk.io/vuln/SNYK-JS-LODASHMERGE-173732)

[Affecting **jquery** package, versions **&lt;3.4.0**](https://snyk.io/vuln/SNYK-JS-JQUERY-174006)\*\*\*\*

### How to prevent

1. Freeze the prototype— use `Object.freeze (Object.prototype)`.
2. Require schema validation of JSON input.
3. Avoid using unsafe recursive merge functions.
4. Consider using objects without prototypes \(for example, `Object.create(null)`\), breaking the prototype chain and preventing pollution.
5. As a best practice use `Map` instead of `Object`.

{% file src="../.gitbook/assets/javascript\_prototype\_pollution\_attack\_in\_nodejs.pdf" %}

