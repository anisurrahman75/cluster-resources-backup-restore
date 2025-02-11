# Backup
- Map with `[group/version/kind]`
- Preferred Version for Restore: 
  - Restore cluster must exist this version CRDS
- Backup Types:
  - Cluster Scope
  - Namespace Scope
  - Application
- Flags:
  - `--exclude-cluster-scoped-resources stringArray`       Cluster-scoped resources to exclude from the backup
  - `--exclude-namespace-scoped-resources stringArray`     Namespaced resources to exclude from the backup
  - `--exclude-namespaces stringArray`                     Namespaces to exclude from the backup
  - `--exclude-resources stringArray`                      Resources to exclude from the backup
  - `--include-cluster-resources optionalBool`             Include cluster-scoped resources in the backup
  - `--include-cluster-scoped-resources stringArray`       Cluster-scoped resources to include in the backup
  - `--include-namespace-scoped-resources stringArray`     Namespaced resources to include in the backup
  - `--include-resources stringArray`                      Resources to include in the backup
  - `--or-selector orLabelSelector`                        Backup resources matching at least one of the label selector from the list
  - `--ordered-resources string`                           Mapping Kinds to an ordered list of specific resources of that Kind
  - `--selector labelSelector`                             Only back up resources matching this label selector

### **Some Example which solves user requirements:**

**Backup an Application's Resources Including Dependencies**:

```yaml
Backup Type: Application-scoped 
Args:
  --include-cluster-resources=true \
  --selector=<>
Customize:
  --include-resources=pv,pvc,secrets,configmaps \
  --selector=<>
```

**Backup All Resources in a Specific Namespace**:
```yaml
Backup Type: Namespace-scoped
Args:
  --exclude-resources=secrets,configmaps \
  --include-cluster-resources=true
```
**Backup Specific Namespaced Resources Across Multiple Namespaces**
```yaml
Backup Type: Cluster
Args:
  --include-namespace-scoped-resources=deployments.apps,services \
  --exclude-namespaces=kube-system,default
```
**Backup Specific Resources Ordered by Priority**
```yaml
Backup Type: Cluster,Namespace
  --include-resources=deployments.apps,statefulsets.apps,daemonsets.apps \
  --ordered-resources="deployments.apps=nginx-deployment,my-deployment statefulsets.apps=my-statefulset daemonsets.apps=my-daemonset"
```
**Backup All CRDs of a Specific Group**
```yaml
Backup Type: Cluster
Args:
  --include-resources=customresourcedefinitions.apiextensions.k8s.io
```
**Backup CR and CRDS**
```yaml
Backup Type: Cluster
Args:
  --include-resources=backupstorages.storage.kubestash.com \
  --selector=apiGroup=storage.kubestash.com
```

## Restore

**Order of restore resource**

- Cluster-Scoped Resources: 
  - Custom Resource Definitions (CRDs)
  - ClusterRoles
  - ClusterRoleBindings
  - Other cluster-scoped resources
  - PV
- Namespace Resources :
  - Namespaces
  - PVCs
  - Namespace-scoped resources (e.g., Workloads, Services, ConfigMaps, Secrets)
  - CRs
  - Any remaining resources
- Flags: 
  - `--exclude-namespaces stringArray`                  Namespaces to exclude from the restore
  - `--exclude-resources stringArray`                   Resources to exclude from the restore
  - `--existing-resource-policy string`                 Restore Policy to be used during the restore workflow, can be - none or update
  - `--include-cluster-resources optionalBool[=true]`   Include cluster-scoped resources in the restore
  - `--include-namespaces stringArray`                  Namespaces to include in the restore
  - `--include-resources stringArray`                   Resources to include in the restore, formatted as `resource.group`
  - `--labels mapStringString`                          Labels to apply to the restore
  - `--namespace-mappings mapStringString`              Namespace mappings from name in the backup to desired restored name in the form src1:dst1,src2:dst2,...
  - `--or-selector orLabelSelector`                     Restore resources matching at least one of the label selector from the list
  - `--preserve-nodeports optionalBool[=true]`          Whether to preserve nodeports of Services when restoring
  - `-l, --selector labelSelector`                      Only restore resources matching this label selector. (default <none>)

### **Some Example which solves user requirements:**

**Restore an Application's Resources Including Dependencies**:

