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