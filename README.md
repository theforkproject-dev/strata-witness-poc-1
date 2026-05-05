# Strata Witness POC 1

Public Tinfoil config repo for the official Strata S3-backed witness POC.

This deployment is intentionally scoped to one L1 mechanical witness:

- Tinfoil-attested runtime
- enclave-generated witness signing key
- gateway-signed `turnstile.witness-sign-request.v1`
- S3 Object Lock guard log before signing
- signed Witness Registry Epoch loaded from this repo

Bootstrap sequence:

1. Deploy this config to Tinfoil.
2. Fetch `/v1/witness-info` through Tinfoil verification.
3. Publish `registry-epoch.json` and `registry-trust-anchors.json` binding the generated public key.
4. Send a gateway-signed request to `/v1/sign-request`.
5. Verify the returned S3 guard evidence.

The registry JSON files are intentionally not present at initial deployment. Until they are published, `/v1/sign-request` returns `WITNESS_REGISTRY_UNAVAILABLE`, while `/health`, `/v1/public-key`, and `/v1/witness-info` remain available for bootstrap.
