# Restore private image pulls

The first deployment on CVM 0.14.7 fails with registry `unauthorized`, including after a dedicated read-only credential successfully retrieves the exact pinned manifest directly. Upstream fixes this process-boundary bug in [5cbe071](https://github.com/tinfoilsh/cvmimage/commit/5cbe07152949366bf103128b12a3aa0c44f92c3c), included in CVM 0.14.9: the container manager must explicitly load the credentials written by the separate boot process.

1. Check 0.14.9 compatibility with the existing configuration and pinned SNP verifier.
2. Change only the CVM version and its explanatory comment. Retain the worker image digest, resource shape, egress restrictions, secret names and production security settings.
3. Independently review the diff, publish a new measured tag, and verify its attestation against the exact source/configuration bytes.
4. Issue a new signed revision under the original fixed solo window, update witnesses and bindings, and retry the existing instance.
5. Require the production attestation and synthetic-job probe before distributing the app. Do not extend the signing window or reuse the old measurement.

Validation: old runtime's live pull fails; replacement credential's exact-manifest read passes; upstream regression tests cover the credential-directory boundary. Local configuration checks and independent source review precede the new measurement. The live pull and production probe are the acceptance checks for the new runtime.
