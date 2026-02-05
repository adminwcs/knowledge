---
products: 
  - Alauda Container Platform
kind:
  - Solution
---

# ACP Integration with CAS Single Sign-On - Solution S2

## Background

If the customer-side identity authentication integration supports CAS, this solution can be used.

## Applicable Version

4.2.1

## Operation Steps

### Step 1: Modify the following parameter values to match the actual CAS service configuration

```json
{"casURI": "https://cas.example.org:8443/cas","login_path": "login"}
```

Parameter Description

| Field                   | Description                                           |
| ----------------------- | ----------------------------------------------------- |
| `casURI`                | Base URL of the CAS server                            |
| `login_path`            | Login path, default is "login"                        |
| `logout_path`           | Logout path, default is "logout"                      |
| `validate_path`         | Ticket validation path, default is "validate"         |
| `service_validate_path` | Service validation path, default is "serviceValidate" |

### Step 2: Encode the JSON configuration in base64



```shell
➜ echo -n '{"casURI": "https://cas.example.org:8443/cas","login_path": "login"}' | base64 -w 0
eyJjYXNVUkkiOiAiaHR0cHM6Ly9jYXMuZXhhbXBsZS5vcmc6ODQ0My9jYXMiLCJsb2dpbl9wYXRoIjogImxvZ2luIn0=
```



### Step 3: Create the CAS Connector resource

Create this resource in the global cluster.

```yaml
apiVersion: dex.coreos.com/v1
config: eyJjYXNVUkkiOiAiaHR0cHM6Ly9jYXMuZXhhbXBsZS5vcmc6ODQ0My9jYXMiLCJsb2dpbl9wYXRoIjogImxvZ2luIn0=                # Replace with the base64 encoding generated in Step 2.
id: cas
kind: Connector
metadata:
  name: cas
  namespace: cpaas-system
name: cas登录
type: cas
```

## Verification Steps

Access the ACP platform, select CAS login, and successfully jump into the platform to verify that the configuration is effective.
