**IMPORTANT:** Frontend side of the project is currently undergoing migration from [AngularJS](https://angularjs.org/) to the current version of [Angular](https://angular.io/). This has caused the project to fall behind in releases. In order to catch up, we will skip dashboard **1.9** and release a **1.10** version.

Moving forward the dashboard community plans to support only the 3 latest stable versions of Kubernetes.

| Dashboard \ Kubernetes | 1.8 | 1.9 | 1.10 | 1.11 |
|------------------------|-----|-----|-----|-----|
| **1.8**                | ✓   | ✓   | ✕   | ✕   |
| **1.10**               | ✕   | ?   | ✓   | ?   |
| **HEAD**               | ✕   | ?   | ✓   | ?   |

- `✓` Fully supported version range.
- `?` Due to breaking changes between Kubernetes API versions, some features might not work in Dashboard (logs, search
etc.).
- `✕` Unsupported version range.