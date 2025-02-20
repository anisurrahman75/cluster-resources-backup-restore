`--exclude-cluster-scoped-resources` stringArray       Cluster-scoped resources to exclude from the backup, formatted as resource.group, such as storageclasses.storage.k8s.io(use '*' for all resources). Cannot work with include-resources, exclude-resources and include-cluster-resources.

`--exclude-namespace-scoped-resources` stringArray     Namespaced resources to exclude from the backup, formatted as resource.group, such as deployments.apps(use '*' for all resources). Cannot work with include-resources, exclude-resources and include-cluster-resources.

`--exclude-namespaces` stringArray                     Namespaces to exclude from the backup.

`--exclude-resources` stringArray                      Resources to exclude from the backup, formatted as resource.group, such as storageclasses.storage.k8s.io. Cannot work with include-cluster-scoped-resources, exclude-cluster-scoped-resources, include-namespace-scoped-resources and exclude-namespace-scoped-resources.

`--include-cluster-resources` optionalBool[=true]      Include cluster-scoped resources in the backup. Cannot work with include-cluster-scoped-resources, exclude-cluster-scoped-resources, include-namespace-scoped-resources and exclude-namespace-scoped-resources.

`--include-cluster-scoped-resources` stringArray       Cluster-scoped resources to include in the backup, formatted as resource.group, such as storageclasses.storage.k8s.io(use '*' for all resources). Cannot work with include-resources, exclude-resources and include-cluster-resources.

`--include-namespace-scoped-resources` stringArray     Namespaced resources to include in the backup, formatted as resource.group, such as deployments.apps(use '*' for all resources). Cannot work with include-resources, exclude-resources and include-cluster-resources.

`--include-namespaces` stringArray                     Namespaces to include in the backup (use '*' for all namespaces). (default *)

`--include-resources` stringArray                      Resources to include in the backup, formatted as resource.group, such as storageclasses.storage.k8s.io (use '*' for all resources). Cannot work with include-cluster-scoped-resources, exclude-cluster-scoped-resources, include-namespace-scoped-resources and exclude-namespace-scoped-resources.

`--labels` mapStringString                             Labels to apply to the backup.

`--or-selector` orLabelSelector                        Backup resources matching at least one of the label selector from the list. Label selectors should be separated by ' or '. For example, foo=bar or app=nginx
      
`--ordered-resources` string                           Mapping Kinds to an ordered list of specific resources of that Kind.  Resource names are separated by commas and their names are in format 'namespace/resourcename'. For cluster scope resource, simply use resource name. Key-value pairs in the mapping are separated by semi-colon.  Example: 'pods=ns1/pod1,ns1/pod2;persistentvolumeclaims=ns1/pvc4,ns1/pvc8'.  









## Restore Sanitize:

1. Metadata: case "generateName", "selfLink", "uid", "resourceVersion", "generation", "creationTimestamp", "deletionTimestamp","deletionGracePeriodSeconds", "ownerReferences":
			delete(metadata, k)

2. Status: unstructured.RemoveNestedField(obj.UnstructuredContent(), "status")
