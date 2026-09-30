# 🌐 Secure AWS Static Website with CloudFront, S3 & GitHub Actions OIDC

[![AWS S3](https://img.shields.io/badge/AWS-S3%20Bucket-569A31?logo=amazon-s3)](https://aws.amazon.com/s3/)
[![CloudFront CDN](https://img.shields.io/badge/CDN-Amazon%20CloudFront-orange?logo=amazon-aws)](https://aws.amazon.com/cloudfront/)
[![CI/CD](https://img.shields.io/badge/CI%2FCD-GitHub%20Actions%20OIDC-blue?logo=github-actions)](https://github.com/KoshaG0hil/aws-static-website/actions)
[![HTTPS Enforced](https://img.shields.io/badge/Security-HTTPS%20Redirect-brightgreen)](https://aws.amazon.com/certificate-manager/)

> **A production-ready static website architecture hosted on Amazon S3, accelerated globally with Amazon CloudFront CDN, and continuously deployed via GitHub Actions using OpenID Connect (OIDC) identity federation.**

---

## 🏛️ Architecture Overview

```
                      +-------------------+
                      |   Global Users    |
                      +---------+---------+
                                |
                                v (HTTPS / TLS 1.3)
                      +-------------------+
                      | Amazon CloudFront | (d2i6b8biog77wq.cloudfront.net)
                      +---------+---------+
                                |
                                v (HTTP/S Website Endpoint)
                      +-------------------+
                      |   Amazon S3       | (koshagohil-static-site-2026)
                      +-------------------+
                                ^
                                | (OIDC aws s3 sync + cache invalidation)
                      +---------+---------+
                      |  GitHub Actions   |
                      +-------------------+
```

---

## 🌟 Key Features

1. **Global CDN Edge Caching**: Low-latency content delivery using Amazon CloudFront distribution `EM66AJ7S4IXFZ`.
2. **Zero Static Credentials (OIDC)**: GitHub Actions assumes AWS IAM Role `GitHubActionsStaticWebsiteDeploymentRole` using dynamic OpenID Connect tokens (`sts:AssumeRoleWithWebIdentity`).
3. **Automated Cache Invalidation**: Every push to `main` automatically syncs website assets to S3 and triggers a CloudFront invalidation for `/*`.
4. **HTTPS Enforced**: CloudFront redirects all HTTP traffic to HTTPS via `redirect-to-https` viewer protocol policy.

---

## 🚀 Deployment Pipeline

The workflow defined in `.github/workflows/deploy.yml` automates:
1. OIDC Token Exchange with AWS STS.
2. Synchronizing static assets: `aws s3 sync . s3://koshagohil-static-site-2026 --delete`.
3. Purging CDN cache: `aws cloudfront create-invalidation --distribution-id EM66AJ7S4IXFZ --paths "/*"`.

---

## 📬 Author

**Kosha Gohil** — Cloud & DevSecOps Engineer  
[GitHub Profile](https://github.com/KoshaG0hil) | [LinkedIn](https://www.linkedin.com/in/koshagohil/)