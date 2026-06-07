# checkov-apra

> **APRA CPS 234 custom policies for [Checkov](https://www.checkov.io)** — scan your Terraform
> for APRA CPS 234 violations *before deploy*. The Australian IaC policy bundle Checkov doesn't ship.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

**Keywords:** Checkov APRA · CPS 234 Terraform · IaC compliance Australia · checkov custom policy APRA

---

## Why this exists

Checkov ships compliance mappings for CIS, PCI, SOC 2, HIPAA and more — but **nothing for the
Australian regulatory context**. These custom policies let APRA-regulated entities catch
CPS 234 (Information Security) violations in Terraform at PR time, shifting compliance left.

## What's in here

`policies/` — 9 declarative (YAML) custom Checkov policies mapped to CPS 234 paragraph 21:

| ID | Control | Resource |
|---|---|---|
| `CKV_APRA_1` | EBS volume encryption | `aws_ebs_volume` |
| `CKV_APRA_2` | RDS storage encryption | `aws_db_instance` |
| `CKV_APRA_3` | RDS not publicly accessible | `aws_db_instance` |
| `CKV_APRA_4` | RDS automated backups | `aws_db_instance` |
| `CKV_APRA_5` | KMS key rotation | `aws_kms_key` |
| `CKV_APRA_6` | CloudTrail multi-region | `aws_cloudtrail` |
| `CKV_APRA_7` | CloudTrail log file validation | `aws_cloudtrail` |
| `CKV_APRA_8` | IAM password policy length ≥ 14 | `aws_iam_account_password_policy` |
| `CKV_APRA_9` | S3 public access block | `aws_s3_bucket_public_access_block` |

## Usage

```bash
pip install checkov
# scan a Terraform directory with the APRA policies
checkov -d path/to/terraform --external-checks-dir checkov-apra/policies
# or only the APRA checks
checkov -d path/to/terraform --external-checks-dir checkov-apra/policies \
        --check CKV_APRA_1,CKV_APRA_2,CKV_APRA_3,CKV_APRA_4,CKV_APRA_5,CKV_APRA_6,CKV_APRA_7,CKV_APRA_8,CKV_APRA_9
```

Drop it into CI to fail PRs that introduce CPS 234 violations.

## Status

✅ **v1 — validated.** 9 YAML policies, loaded + evaluated by Checkov against real Terraform
(the `aiopsone-au-landing-zone` modules). Covers the AWS-detectable CPS 234 para-21 controls;
more (S3 SSE via the split encryption resource, VPC flow logs, GuardDuty) can follow.

## Related

- 🔗 Pairs with the **[aiopsone-au-landing-zone](https://github.com/jaybilgaye/aiopsone-au-landing-zone)** Terraform (scan it in CI).
- 🌐 More at **[aiopsone.com](https://aiopsone.com)**.
