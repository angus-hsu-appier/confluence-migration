---
title: itest-cli Local Testing Guide
source: https://appier.atlassian.net/wiki/spaces/IDASH/pages/4931977327/itest-cli+Local+Testing+Guide
confluence_id: 4931977327
space: IDASH
last_modified: 2026-02-09
author: Ryan Hsieh
migrated_at: 2026-06-03
---

# itest-cli Local Testing Guide

## Overview

The `itest-cli` tool is an API test CLI that:

- Captures/dumps API responses from remote endpoints
- Compares dumped responses with current responses
- Alerts on failures via OpsGenie (optional)
- Stores test data in GCS for distributed test management

---

## 1. Setup

### Download Binary

```bash
mkdir itest && cd itest

# Login to Vault
vault login -method=oidc

# Set binary version (itest-cli-mac-arm64 | itest-cli-mac-amd64 | itest-cli-linux-amd64)
export EXE_VERSION=itest-cli-mac-arm64

# Download binary
export BINARIES_TOKEN=$(vault kv get -field=binaries_token secret/project/trading-desk/itest/itest-config)
curl -fsSL -H "Authorization: Bearer $BINARIES_TOKEN" -H "Accept: application/vnd.github.v3.raw" "https://api.github.com/repos/plaxieappier/itest-cli/contents/$EXE_VERSION?ref=release" --output itest-cli
chmod +x itest-cli
```

### Prepare Config Files

Download these template files to your `itest` directory:

| File | Source |
|------|--------|
| `itest-config.yaml.tpl` | [td-domain](https://github.com/plaxieappier/td-domain/blob/master/packages/itest-regular-test/files/itest-config.yaml.tpl) |
| `gcp_service_account.json.tpl` | [td-domain](https://github.com/plaxieappier/td-domain/blob/master/packages/itest-regular-test/files/gcp_service_account.json.tpl) |
| `testcases.yaml` | [trading-desk-monitor](https://github.com/plaxieappier/trading-desk-monitor/blob/master/itest-testcase/idash-api-server-prd-manual-test.yaml) |

Render config files with consul-template:

```bash
export VAULT_ADDR="https://vault.appier.us/"

# Render itest-config.yaml
consul-template -once -vault-addr ${VAULT_ADDR} -template itest-config.yaml.tpl:itest-config.yaml -vault-renew-token=false

# Fix template function and render service account
sed -i '' 's/toJson/toJSON/g' gcp_service_account.json.tpl
consul-template -once -vault-addr ${VAULT_ADDR} -template gcp_service_account.json.tpl:service_account.json -vault-renew-token=false
```

### Final Directory Structure

```
itest/
├── itest-cli                    # Binary
├── itest-config.yaml            # Config with credentials & URLs
├── service_account.json         # GCS access credentials
└── testcases.yaml               # Test case definitions
```

---

## 2. Running Tests

!!! warning "Local/Staging Testing"
    Set OpsGenie to disabled in `itest-config.yaml` before running tests locally or against staging, to avoid triggering real alerts:

    ```yaml
    opsGenie:
      enabled: false
    ```

### From GCS (Recommended)

```bash
# Staging (default for local testing)
./itest-cli gcs-run --bucketPath gs://appier-idash-itest-testcase/idash-api-server-stg-manual-test --serviceAccount service_account.json

# Production
./itest-cli gcs-run --bucketPath gs://appier-idash-itest-testcase/idash-api-server-prd-manual-test --serviceAccount service_account.json

# AI-Bidding (staging)
./itest-cli gcs-run --bucketPath gs://appier-idash-itest-testcase/ai-bidding-idash-api-server-stg-manual-test --serviceAccount service_account.json

# AI-Bidding (production)
./itest-cli gcs-run --bucketPath gs://appier-idash-itest-testcase/ai-bidding-idash-api-server-prd-manual-test --serviceAccount service_account.json
```

### From Local Files

**Get test files** (choose one method):

```bash
# Method 1: Download from GCS
mkdir -p ./local-tests
gsutil -o "GSUtil:parallel_process_count=1" cp -r gs://appier-idash-itest-testcase/idash-api-server-stg-manual-test ./local-tests/

# Method 2: Generate from YAML config
./itest-cli gcs-add --serviceAccount service_account.json --bucketPath gs://appier-idash-itest-testcase/idash-api-server-stg-manual-test --localConfig testcases.yaml
# Files generated in ./itest-tmp/
```

**Run tests:**

```bash
./itest-cli run --path ./local-tests --recursive
```

### Add Single GET Test

```bash
./itest-cli add --env staging --path /api/endpoint/123 [--force]
```

> Note: GET only, creates response snapshot file, does not upload to GCS.

---

## 3. Updating Test Data

### Step 1: Delete Existing Files in GCS

`gcs-add` does NOT auto-overwrite. Delete existing files first:

- [Staging bucket](https://console.cloud.google.com/storage/browser/appier-idash-itest-testcase/idash-api-server-stg-manual-test)
- [Production bucket](https://console.cloud.google.com/storage/browser/appier-idash-itest-testcase/idash-api-server-prd-manual-test)

### Step 2: Upload to GCS

```bash
# Staging
./itest-cli gcs-add --serviceAccount service_account.json --bucketPath gcs://appier-idash-itest-testcase/idash-api-server-stg-manual-test --localConfig testcases.yaml

# Production
./itest-cli gcs-add --serviceAccount service_account.json --bucketPath gcs://appier-idash-itest-testcase/idash-api-server-prd-manual-test --localConfig testcases.yaml

# AI-Bidding (staging)
./itest-cli gcs-add --serviceAccount service_account.json --bucketPath gcs://appier-idash-itest-testcase/ai-bidding-idash-api-server-stg-manual-test --localConfig ai-bidding-testcases-stg.yaml

# AI-Bidding (production)
./itest-cli gcs-add --serviceAccount service_account.json --bucketPath gcs://appier-idash-itest-testcase/ai-bidding-idash-api-server-prd-manual-test --localConfig ai-bidding-testcases-prd.yaml
```

---

## 4. Test Case YAML Format

```yaml
schemaVersion: v1
method: GET|POST
env: staging|production
pathTemplate: /api/endpoint/{PARAM}
ignoreRules:
  - extensions.tracing
  - field.to.ignore
params:
  paramName:
    - value1
    - value2
payloads:             # For POST requests only
  - |
    { "key": "value" }
```

**Generated files:**

- `{env}__{base64_path}.json` - API response snapshot
- `{env}__{base64_path}.in.json` - Test case metadata

---

## 5. Reference

### Quick Commands

| Task | Command |
|------|---------|
| Run local tests | `./itest-cli run --path ./folder --recursive` |
| Run from GCS | `./itest-cli gcs-run --bucketPath gs://...bucket-path --serviceAccount sa.json` |
| Upload to GCS | `./itest-cli gcs-add --serviceAccount sa.json --bucketPath gcs://...bucket-path --localConfig testcases.yaml` |
| Add single GET test | `./itest-cli add --env staging --path /api/endpoint` |

### GCS Buckets

| Environment | Path |
|-------------|------|
| Staging | `gs://appier-idash-itest-testcase/idash-api-server-stg-manual-test` |
| Production | `gs://appier-idash-itest-testcase/idash-api-server-prd-manual-test` |
| AI-Bidding Staging | `gs://appier-idash-itest-testcase/ai-bidding-idash-api-server-stg-manual-test` |
| AI-Bidding Production | `gs://appier-idash-itest-testcase/ai-bidding-idash-api-server-prd-manual-test` |

### Template Sources

| File | Repository |
|------|------------|
| `itest-config.yaml.tpl` | [td-domain](https://github.com/plaxieappier/td-domain/blob/master/packages/itest-regular-test/files/itest-config.yaml.tpl) |
| `gcp_service_account.json.tpl` | [td-domain](https://github.com/plaxieappier/td-domain/blob/master/packages/itest-regular-test/files/gcp_service_account.json.tpl) |
| `testcases.yaml` (prd) | [trading-desk-monitor](https://github.com/plaxieappier/trading-desk-monitor/blob/master/itest-testcase/idash-api-server-prd-manual-test.yaml) |

---

## Changelog

| Version | Changes |
|---------|---------|
| v4 | Added ai-bidding test examples (run & upload). Added OpsGenie local/staging test warning. |
| v3 | Simplified from 415 to ~200 lines. Removed manual Vault+jq method, git clone alternative, and verbose explanations. Consolidated running tests into GCS/Local sections. |
| v2 | Added consul-template method, aligned with README |
| v1 | Initial guide with manual Vault configuration |
