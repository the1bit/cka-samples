# CKA Practice: Nginx ConfigMap TLS1.2

## Official Documentation Reference

- [Configure a Pod to Use a ConfigMap](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/)
- [Configure TLS for NGINX Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/#tls)

## Task

**Update the Nginx ConfigMap to use TLS1.2**

## Initial Example ConfigMap (using TLSv1 and TLSv1.1)

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
    name: nginx-config
    namespace: default
data:
    nginx.conf: |
        events {}
        http {
            server {
                listen              443 ssl;
                ssl_certificate     /etc/nginx/tls/tls.crt;
                ssl_certificate_key /etc/nginx/tls/tls.key;
                ssl_protocols       TLSv1 TLSv1.1;
                # other settings...
            }
        }
```

## Update Step

Edit the ConfigMap to use only TLS1.2:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
    name: nginx-config
    namespace: default
data:
    nginx.conf: |
        events {}
        http {
            server {
                listen              443 ssl;
                ssl_certificate     /etc/nginx/tls/tls.crt;
                ssl_certificate_key /etc/nginx/tls/tls.key;
                ssl_protocols       TLSv1.2;
                # other settings...
            }
        }
```

## Steps

1. **Edit the ConfigMap**  
   Use `kubectl edit configmap nginx-config` or update the YAML as above.

2. **Reload Nginx**  
   If Nginx runs as a Deployment, restart the pods to apply changes:
   ```sh
   kubectl rollout restart deployment <nginx-deployment-name>
   ```

## Key Points for CKA

- Know how to edit a ConfigMap.
- Understand how to set `ssl_protocols TLSv1.2;` in Nginx config.
- Be able to reload/restart pods to apply config changes.

## Practice

- Start with a ConfigMap using older TLS versions.
- Update it to use only TLS1.2.
- Mount it to an Nginx pod.
- Verify Nginx uses TLS1.2 (check config, logs, or test with `openssl`).

---

**Tip:** Always refer to the [Kubernetes ConfigMap documentation](https://kubernetes.io/docs/concepts/configuration/configmap/) for syntax and usage.

