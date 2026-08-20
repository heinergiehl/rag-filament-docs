# Release Notes v1.4.1

Filament RAG 1.4.1 adds first-class Laravel 13 compatibility and fixes the Windows sandbox installer used to validate clean package installations.

## Laravel 13 Support

- Supports Laravel 12.61.1+ and Laravel 13.x.
- Uses the matching package-test lanes: Testbench 10 for Laravel 12 and Testbench 11 for Laravel 13.
- Laravel 13 installations resolve Filament 5.7+ and Laravel AI 0.7+ through Composer.
- The release was verified in a new Laravel 13 application with package discovery, Filament panel installation, pgvector migrations, the built-in Doctor, RAG admin routes, and the authenticated HTTP boundary.

## Installer Fix

The Windows PowerShell sandbox and smoke installers no longer pass a caret constraint through `composer.bat`. That caret was stripped by the Windows command boundary and could make Composer request exact Filament 5.2, which is both incompatible with the package and blocked by current security advisories.

New sandbox and smoke applications now:

- default to Laravel `13.*`;
- request the current Filament 5 line with a Windows-safe constraint;
- keep the Laravel constraint configurable when maintainers need to exercise Laravel 12.

## Upgrade Steps

1. Update the package within the 1.4 release line:

   ```bash
   composer require heiner/filament-rag:"1.4.*" --with-all-dependencies
   ```

2. Publish current Filament assets and restart long-running workers:

   ```bash
   php artisan filament:assets
   php artisan queue:restart
   ```

3. Run the readiness check:

   ```bash
   php artisan filament-rag:doctor
   ```

Version 1.4.1 contains no new database migration. Existing bots, sources, documents, chunks, conversations, and messages are unchanged. A fresh installation still runs the normal package migrations once.

## Compatibility

- PHP 8.4+
- Laravel 12.61.1+ or 13.x
- Filament 5.6.5+; Composer selects 5.7+ on Laravel 13
- Laravel AI 0.1.5+, 0.6.7+, or 0.7.x; Composer selects 0.7+ on Laravel 13

