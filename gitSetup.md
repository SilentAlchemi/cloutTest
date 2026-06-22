# Git Setup

Minimal SSH-based setup for pushing this folder to GitHub.

## Repo

From this folder:

```bash
git init
git add .
git commit -m "Initial cloud test"
git branch -M main
```

## Existing SSH Key

Check local SSH files:

```bash
dir %USERPROFILE%\.ssh
```

If you already have a public key, show it:

```bash
type %USERPROFILE%\.ssh\id_rsa.pub
```

## Add Key To GitHub

In GitHub as the correct account:

1. `Settings`
2. `SSH and GPG keys`
3. `New SSH key`
4. paste the public key

## Set GitHub Remote

```bash
git remote add origin git@github.com:SilentAlchemi/cloutTest.git
```

If `origin` already exists:

```bash
git remote set-url origin git@github.com:SilentAlchemi/cloutTest.git
```

## Test SSH

```bash
ssh -T git@github.com
```

Expected result:

```text
Hi SilentAlchemi! You've successfully authenticated, but GitHub does not provide shell access.
```

## Push

```bash
git push -u origin main
```

After that, future updates are:

```bash
git add .
git commit -m "Update"
git push
```

## DreamHost Auto Deploy

This repo includes:

- `.github/workflows/deploy.yml`

It deploys `main` to DreamHost over SSH.

### DreamHost Target

Current live path:

```text
/home/dh_i4cazx/birthstore.club/cloutTest
```

Current host:

```text
vps44966.dreamhostps.com
```

### Recommended Deploy Key

Use a separate SSH key for GitHub Actions, not your personal machine key.

Generate one locally:

```bash
ssh-keygen -t ed25519 -C "github-actions-dreamhost" -f %USERPROFILE%\.ssh\clouttest_dreamhost
```

This creates:

- `%USERPROFILE%\.ssh\clouttest_dreamhost`
- `%USERPROFILE%\.ssh\clouttest_dreamhost.pub`

Show the public key:

```bash
type %USERPROFILE%\.ssh\clouttest_dreamhost.pub
```

### Add Deploy Key To DreamHost

SSH into DreamHost:

```bash
ssh dh_i4cazx@vps44966.dreamhostps.com
```

Append the public key to:

```text
~/.ssh/authorized_keys
```

Fast way on the server:

```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
nano ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```

Paste the public key as a new line and save.

### GitHub Repo Secrets

In GitHub repo:

1. `Settings`
2. `Secrets and variables`
3. `Actions`
4. `New repository secret`

Create these secrets:

- `DREAMHOST_HOST`
  - `vps44966.dreamhostps.com`
- `DREAMHOST_USER`
  - `dh_i4cazx`
- `DREAMHOST_TARGET_PATH`
  - `/home/dh_i4cazx/birthstore.club/cloutTest`
- `DREAMHOST_SSH_PRIVATE_KEY`
  - contents of `%USERPROFILE%\.ssh\clouttest_dreamhost`

Show the private key for copy/paste:

```bash
type %USERPROFILE%\.ssh\clouttest_dreamhost
```

### Test Deploy

Push to `main`:

```bash
git add .
git commit -m "Test deploy"
git push
```

Then check:

1. GitHub `Actions` tab
2. workflow `Deploy To DreamHost`
3. live page:

```text
https://birthstore.club/cloutTest/
```
