|                    | Kubernetes 1.4 | Kubernetes 1.5 | Kubernetes 1.6 | Kubernetes 1.7 | Kubernetes 1.8 |
|--------------------|----------------|----------------|----------------|----------------|----------------|
| **Dashboard 1.4**  | ✓              | ✕              | ✕              | ✕              | ✕              |
| **Dashboard 1.5**  | ✕              | ✓              | ✕              | ✕              | ✕              |
| **Dashboard 1.6**  | ✕              | ✕              | ✓              | ?              | ✕              |
| **Dashboard 1.7**  | ✕              | ✕              | ?              | ✓              | ✕              |
| **Dashboard HEAD** | ✕              | ✕              | ✕              | ✓              | ✓              |

- `✓` Fully supported version range.
- `?` Due to breaking changes between Kubernetes API versions, some features might not work in Dashboard (logs, search
etc.).
- `✕` Unsupported version range.