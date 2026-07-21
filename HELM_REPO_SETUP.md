# Helm Chart Repository Setup

## Create GitHub Pages Helm Repo

### Option 1: Using `docs/` folder (recommended)

```bash
# 1. Create docs directory
mkdir -p docs

# 2. Copy packaged chart
cp charts/nfs-subdir-external-provisioner-4.0.19.tgz docs/

# 3. Generate index
helm repo index docs --url https://abeytom07.github.io/nfs-subdir-external-provisioner/

# 4. Commit and push
git add docs/
git commit -m "Add Helm chart repository"
git push origin master
```

### Option 2: Using `gh-pages` branch

```bash
# 1. Create orphan branch
git checkout --orphan gh-pages
git rm -rf .

# 2. Copy chart
cp charts/nfs-subdir-external-provisioner-4.0.19.tgz .

# 3. Generate index
helm repo index . --url https://abeytom07.github.io/nfs-subdir-external-provisioner/

# 4. Commit and push
git add .
git commit -m "Initial Helm chart repository"
git push origin gh-pages

# 5. Switch back
git checkout master
```

## Enable GitHub Pages

1. Go to: https://github.com/abeytom07/nfs-subdir-external-provisioner/settings/pages
2. Source: 
   - Option 1: Deploy from `master` branch → `/docs` folder
   - Option 2: Deploy from `gh-pages` branch → `/ (root)`
3. Click **Save**

Wait 2-3 minutes for deployment.

## Use the Repository

```bash
helm repo add nfs-subdir-external-provisioner https://abeytom07.github.io/nfs-subdir-external-provisioner/
helm repo update
helm search repo nfs-subdir-external-provisioner
```

## Install from Repository

```bash
helm install nfs-provisioner nfs-subdir-external-provisioner/nfs-subdir-external-provisioner \
  --set nfs.server=YOUR_NFS_SERVER \
  --set nfs.path=/your/path \
  --namespace nfs-provisioner \
  --create-namespace
```

## Update Chart (Future)

```bash
# After building new chart version
cp charts/nfs-subdir-external-provisioner-*.tgz docs/
helm repo index docs --url https://abeytom07.github.io/nfs-subdir-external-provisioner/ --merge docs/index.yaml
git add docs/
git commit -m "Update Helm chart to vX.X.X"
git push origin master
```
