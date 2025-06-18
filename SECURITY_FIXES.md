Vulnerability Scanning and Mitigation

## Tool Used
- Trivy (v0.62.0)

## Original Vulnerability Scan
- File: `trivy-report.txt`
- Notable Critical Finding: 
  - `GHSA-rvg8-pwq2-xj7q` (base64url@0.0.6)
  - Source: Transitive dependency of `express-jwt@0.1.3`

## Fixes Applied
- Upgraded `express-jwt` from `0.1.3` → `7.7.5`
- Upgraded `jsonwebtoken` from `0.1.0` → `9.0.0`
- Removed all transitive usage of `base64url@0.0.6`

## Final Scan Result
- File: `trivy-report-after-final.txt`
- Confirmed: No critical vulnerabilities remain
- `base64url@0.0.6` no longer present in the dependency tree

## Notes
- These changes required also upgrading TypeScript types for compatibility in the container build.
- Final Docker image is fully rebuilt and secured.
