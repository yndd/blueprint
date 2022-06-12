# core

## Description
sample description

## Usage

### Fetch the package
`kpt pkg get REPO_URI[.git]/PKG_PATH[@VERSION] core`
kpt pkg get https://github.com/yndd/blueprint/core@v0.0.2
Details: https://kpt.dev/reference/cli/pkg/get/

### View package content
`kpt pkg tree core`
Details: https://kpt.dev/reference/cli/pkg/tree/

### Additional transformers

If you want to change the image names add these additional files

REGSITRY=<new registry>

```
cat <<EOF >> core/TransformConfigmapCore.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: transform-image-core
data:
  name: yndd/nddcore
  newName: ${REGISTRY}/nddcore
  newTag: latest
EOF
```

```
cat <<EOF >> core/TransformConfigmapRbac.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: transform-image-rbac
data:
  name: yndd/nddrbac
  newName: ${REGISTRY}/nddrbac
  newTag: latest
EOF
```

Afterwards render the output

```
kpt fn render core
```
### Apply the package
```
kpt live init core
kpt live apply core --reconcile-timeout=2m --output=table
```
Details: https://kpt.dev/reference/cli/live/
