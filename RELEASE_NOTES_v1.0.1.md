# Release Notes - v1.0.1 (2026-02-24)

## Summary

`v1.0.1` is a small compatibility/operations patch release.
It aligns the supported PHP versions with the plugin's upstream dependencies and prevents accidental demo deployments.

## Changed

- Require PHP `8.4+` (dependency `laravel/ai`) and align CI/docs to supported versions.
- Make demo deploy workflow manual-only to prevent unintended deployments.

## Fixed

- PHPStan CI config: mark excluded paths optional when directories are absent in the runner.

## Upgrade Notes

- If you are running PHP `8.3` or older, you must upgrade to PHP `8.4+` before updating to `v1.0.1`.
