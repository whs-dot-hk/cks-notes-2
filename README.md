```sh
k run
k deploy
k expose
k scale
```
# Kubeadm
```sh
kubeadm upgrade plan
kubeadm upgrade apply
```
# Apparmor
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: hello-apparmor
  annotations:
    # Tell Kubernetes to apply the AppArmor profile "k8s-apparmor-example-deny-write".
    # Note that this is ignored if the Kubernetes node is not running version 1.4 or greater.
    container.apparmor.security.beta.kubernetes.io/hello: localhost/k8s-apparmor-example-deny-write
spec:
  containers:
  - name: hello
    image: busybox:1.28
    command: [ "sh", "-c", "echo 'Hello AppArmor!' && sleep 1h" ]
```

# Label
## Namespace
https://kubernetes.io/docs/reference/labels-annotations-taints/#kubernetes-io-metadata-name
```yaml
kubernetes.io/metadata.name: "mynamespace"
```
## `matchLabels` and `matchExpressions`
```yaml
selector:
  matchLabels:
    component: redis
  matchExpressions:
    - {key: tier, operator: In, values: [cache]}
    - {key: environment, operator: NotIn, values: [dev]}
```
# Configmaps
```yaml
  volumes:
    - name: config-volume
      configMap:
        # Provide the name of the ConfigMap containing the files you want
        # to add to the container
        name: special-config
```
```yaml
      envFrom:
        - configMapRef:
            name: special-config
```
```yaml
          valueFrom:
            configMapKeyRef:
              name: special-config
              key: SPECIAL_LEVEL
```
# Secrets
```yaml
  volumes:
  - name: foo
    secret:
      secretName: mysecret
```
```yaml
      envFrom:
      - secretRef:
          name: mysecret
```
```yaml
        valueFrom:
          secretKeyRef:
            name: mysecret
            key: username
```
# Auditing
* `--audit-log-maxage`
* `--audit-log-maxbackup`
* `--audit-log-maxsize`
# `automountServiceAccountToken`
```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: build-robot
automountServiceAccountToken: false
```
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  serviceAccountName: build-robot
  automountServiceAccountToken: false
```
# Security context
# Network policy
# Admission controller
```sh
--enable-admission-plugins=...
```
