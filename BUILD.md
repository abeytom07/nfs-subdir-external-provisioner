# Build Instructions

## Prerequisites
- Go 1.27.2+
- Docker

## Build Binary

```bash
go mod vendor
make build
```

Binaries in `bin/` directory.

## Package & Deploy

```bash
export V=4.0.203
```

### Build Multiarch Image

```bash
docker buildx build --platform linux/amd64,linux/arm64,linux/arm/v7 \
  -t abeytom07d/nfs-subdir-external-provisioner:${V} \
  -f Dockerfile.multiarch --push .
```

```bash
docker buildx build --platform linux/amd64 \
  -t abeytom07d/nfs-subdir-external-provisioner:${V} \
  -f Dockerfile.multiarch --push .
```

### Package Helm Chart

```bash
helm package charts/nfs-subdir-external-provisioner --version=$V --app-version=$V -d bin/
```

### Deploy Helm Chart

```bash
git checkout gh-pages
cp bin/nfs-subdir-external-provisioner-${V}.tgz .
helm repo index . --url https://abeytom07.github.io/nfs-subdir-external-provisioner/
git commit -m "Helm chart of $V"
git push
```
