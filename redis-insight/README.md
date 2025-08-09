# Redis Insight Binary Storage

## Overview
This directory contains the Redis Insight installer binaries used by the SDE system.

## Current Version: 2.70.1

**Source:** https://redis.io/insight/  
**Download Date:** August 8, 2025  
**File:** `Redis-Insight-win-installer-2.70.1.exe`  
**SHA256:** `2469d275e93faa6d1af0338265cf60cfe44b2f63dcedd73f9fae798189692df4`  

## Why Mirrored?

Redis Insight is mirrored in scoop-binaries for the following reasons:
1. **Version Pinning** - Ensures consistent version across all SDE installations
2. **Reliability** - Reduces dependency on external download availability
3. **Security** - Allows verification of binary integrity before distribution
4. **Compliance** - Enables security scanning and approval processes

## Usage in Manifests

The binary is referenced in `bucket/redisinsight.json`:

```json
{
  "url": "https://bitbucket.org/riskexec/scoop-binaries/raw/main/redis-insight/2.70.1/Redis-Insight-win-installer-2.70.1.exe",
  "hash": "2469d275e93faa6d1af0338265cf60cfe44b2f63dcedd73f9fae798189692df4"
}
```

## Updating Process

To update to a new version:

1. **Download** the new installer from https://redis.io/insight/
2. **Verify** the binary integrity and scan for security issues
3. **Create** new version directory: `redis-insight/<new-version>/`
4. **Upload** the binary with proper naming: `Redis-Insight-win-installer-<version>.exe`
5. **Compute** SHA256 hash: `Get-FileHash -Algorithm SHA256 <file>`
6. **Update** checksums file: `checksums-<version>.txt`
7. **Update** manifest in bucket repository
8. **Test** end-to-end installation

## License Compliance

Redis Insight is distributed under Redis Labs' license terms. Redistribution is permitted for internal use according to their licensing terms. See: https://redis.io/insight/

## Security Notes

- All binaries should be scanned for malware before addition
- Verify file signatures when available
- Document any security exceptions or approvals
- Maintain audit trail for compliance

---

**Maintainer:** Erik Pieczkowski  
**Last Updated:** August 2025