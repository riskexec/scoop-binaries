# SDE Scoop Binaries

This Git LFS repository provides central, versioned storage for binary artifacts used by the SDE (Standard Desktop Environment) system. It serves as a reliable mirror for software that isn't suitable for direct upstream retrieval.

## Purpose

- **Pinned Versions** - Store specific software versions to prevent breaking changes
- **Vendor-Gated Downloads** - Host software that requires authentication or registration
- **Custom Builds** - Store internally modified or custom-built software
- **Reliable Storage** - Ensure availability independent of upstream sources
- **Deterministic Checksums** - Every binary has SHA256 verification for security

## Repository Structure

### Directory Layout
```
<tool-name>/
  <version>/
    <artifact-files>
    checksums-<version>.txt (optional)
```

### Git LFS Configuration
- **All `.exe` files** are stored using Git Large File Storage
- Configured in `.gitattributes`: `*.exe filter=lfs diff=lfs merge=lfs -text`
- Efficient handling of large binary files

### Current Contents
- `Redis-Insight-win-installer-2.70.1.exe` - Redis management tool installer

## Workflow for Adding Binaries

### 1. Package the Artifact
- Use vendor-provided archives when available
- If re-packaging is necessary, document rationale in tool folder README
- Maintain original signatures and provenance information

### 2. Naming Convention
Use predictable, descriptive names:
```
<tool>-<version>-<platform>.<ext>
```
Examples:
- `redis-insight-2.70.1-win64.exe`
- `custom-tool-1.0.0-windows.zip`

### 3. Compute SHA256 Hash
```powershell
# PowerShell
Get-FileHash -Path <path-to-file> -Algorithm SHA256
```

Store hash as:
- `<artifact>.sha256` file, or
- In versioned `checksums-<version>.txt` file

### 4. Directory Structure
Create appropriate version directory:
```
mkdir <tool-name>/<version>/
```

### 5. Git LFS Commit
```bash
git add <tool-name>/
git commit -m "Add <tool> version <version>"
git push
```

### 6. Reference from Manifests
Update SDE bucket manifests to reference the stable URLs:

```json
{
  "url": "https://bitbucket.org/riskexec-inc/scoop-binaries/raw/main/<tool>/<version>/<artifact>",
  "hash": "<sha256-hash>"
}
```

## Referencing from Manifests

### URL Pattern
```
https://bitbucket.org/riskexec-inc/scoop-binaries/raw/main/<tool>/<version>/<artifact>
```

### Version Synchronization
Keep these in sync across manifest updates:
- Manifest `version` field
- Artifact path in `url`
- SHA256 `hash` value

### Example Manifest Entry
```json
{
  "version": "2.70.1",
  "url": "https://bitbucket.org/riskexec-inc/scoop-binaries/raw/main/redis-insight/2.70.1/Redis-Insight-win-installer-2.70.1.exe",
  "hash": "1a2b3c4d5e6f7890abcdef1234567890abcdef1234567890abcdef1234567890"
}
```

## Retention Policy

- **Keep Multiple Versions** - Maintain at least 2-3 prior versions for rollbacks
- **Deprecation Process** - Mark deprecated versions before removal
- **Cooling Period** - Define time period before removing old versions
- **Change Documentation** - Maintain changelog per tool for behavioral changes

## Security and Compliance

### Verification Steps
1. **Verify vendor signatures** when available before mirroring
2. **Scan for malware** using enterprise security tools
3. **Record SHA256 hashes** for all artifacts
4. **Document provenance** - source, date, verification steps

### Sensitive Data
- **No credentials or license keys** in repository
- **Use placeholders** for sensitive configuration
- **External configuration** for secrets and keys

## Troubleshooting

### Hash Mismatch Errors
```powershell
# Clear cache and retry
scoop cache rm <app>
scoop install <app> -s

# If still failing, recompute hash
Get-FileHash -Path <artifact> -Algorithm SHA256
```

### 404 Errors
- Verify artifact exists in repository at specified path
- Check branch name (should be `main`)
- Ensure proper authentication for private repository access

### Git LFS Issues
```bash
# Verify LFS is installed
git lfs version

# Re-pull LFS objects
git lfs pull
```

## Contributing

When adding new binaries:

1. **Document the need** - Why can't this be sourced directly?
2. **Verify licensing** - Ensure redistribution is permitted
3. **Test thoroughly** - Install from scoop-binaries URL before committing
4. **Update manifests** - Reference the new artifact in appropriate SDE manifests
5. **Document provenance** - Include source, date, and verification notes

---

**Part of:** SDE (Standard Desktop Environment)  
**Maintainer:** Erik Pieczkowski  
**Git LFS Required:** Yes  
**Last Updated:** August 2025