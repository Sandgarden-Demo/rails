---
title: "Release notes v8.2.0.alpha"
date: 2026-01-23
version: "8.2.0.alpha"
---

This release includes updates across Rails 8.2.

## Active Support

*   Updated YAML loading across the framework to require `Psych` >= 4 and to use `YAML.unsafe_load` / `YAML.unsafe_load_file` when unsafe loading is required.

*   Improved `String#squish!` performance by avoiding replacements for single ASCII spaces while still normalizing other whitespace.

*   Fixed `ActiveSupport::EncryptedConfiguration#require` to raise `KeyError` with a descriptive `Missing key: ...` message for missing keys.

*   Improved `File.atomic_write` temporary-file creation by using an exclusive random filename and preserving existing file permissions.

*   Updated Active Support test assertions to strip leading lambda syntax (`->`) from callable source strings.

## Active Model

*   Fixed `ActiveModel::Errors#added?` to ignore `:callback` and `:message` options consistently when comparing stored errors and query options.

## Active Record

*   Added support for `Float` enum values when the underlying column type is a float.

*   Fixed PostgreSQL reconnection and reset handling by reapplying `schema_search_path` and using `libpq` `parameter_status("search_path")` on PostgreSQL 18+ when available.

*   Fixed bulk `change_table` migration reversion to use the provided `table_name` and replay recorded operations in reverse order.

*   Fixed PostgreSQL schema dumping to avoid double-prefixing schema-qualified relation names.

*   Updated uniqueness validation query construction to use Arel case-sensitive and case-insensitive equality operators without requiring a database connection.

*   Updated Arel tree managers to derive the SQL engine from the associated table class for `delete`, `insert`, and `update` managers.

*   Fixed batched association preloading for STI models by grouping batches by both `loader_query` and `klass`.

## Action View

*   Fixed `file_field` `accept:` option arrays to render as a comma-separated attribute value.

*   Fixed streaming rendering to report template errors to `ActiveSupport.error_reporter` when configured and to log otherwise.

## Action Pack

*   Added block support to `ActionController::Parameters#merge` (forwarded to `Hash#merge`) while preserving permitted status.

*   Updated `ActionController::Parameters#fetch` to yield the missing key to the fallback block.

*   Added `config.action_controller.live_streaming_excluded_keys` to exclude execution-state keys from being shared into `ActionController::Live` streaming threads.

*   Added `ActionDispatch::Request#bearer_token` to parse `Authorization: Bearer ...` headers.

*   Fixed `protect_from_forgery prepend: true` to prepend both forgery protection callbacks in the correct order.

*   Deprecated calling `protect_from_forgery` without `:with` and set Rails 8.2 defaults to use `:exception` as the default strategy.

*   Updated forgery protection to skip adding `Sec-Fetch-Site` to the `Vary` header when forgery protection is skipped.

*   Fixed header-only CSRF protection to allow a missing `Sec-Fetch-Site` header on HTTP when SSL is not forced.

*   Removed the `X-XSS-Protection` header from Rails 8.2 default response headers.

## Active Job

*   Deprecated the built-in Backburner, Sneakers, delayed_job, Resque, and queue_classic adapters for removal in Rails 9.0.

*   Updated `retry_on` to allow wait procs to accept the raised error as an optional second argument.

*   Restored `config.active_job.enqueue_after_transaction_commit` as a functional boolean and defaulted it to `true` for new applications.

## Action Mailer

*   No changes.

## Action Cable

*   No changes.

## Active Storage

*   Added configurable checksum algorithms (MD5 or SHA256) for services and direct uploads, centralizing checksum computation and verification in `ActiveStorage::Service`.

*   Restored an overridable IAM client for Google Cloud Storage URL signing with a default based on Application Default Credentials.

## Action Mailbox

*   No changes.

## Action Text

*   No changes.

## Railties

*   Updated the minimum supported Ruby version to 3.3.

*   Added `Rails.app` as an alias for `Rails.application`.

*   Added `Rails.app.creds` to provide combined access to configuration values from `ENV` and encrypted credentials and, in development, `.env`.

*   Added `Rails.app.dotenvs` to provide `.env` lookup with variable and command interpolation.

*   Added `Rails.app.revision` / `config.revision` to expose a deployment revision in `Rails::Info` and error reporting context.

*   Updated `rails notes` to recognize CSS block comments and to trim whitespace around printed notes.

*   Updated tooling parsing to use Prism and removed Ripper-based parsing fallbacks for `render`, `notes`, and test parsing paths.

*   Updated generated CI templates to install `libvips` when Active Storage is enabled.

*   Updated generated CI templates to make system tests opt-in by default.

*   Updated generated templates and devcontainer defaults to standardize Ruby version selection (including Ruby 4.0.0 availability).

*   Fixed token attribute generation to avoid emitting duplicate `unique: true` declarations.

*   Updated test tooling to improve isolation by scoping environment variables (such as `DATABASE_URL`, `RAILS_ENV`, and `RACK_ENV`) to individual tasks.

*   Updated console `reload!` to reset the application executor when active.

## Guides

*   Updated the Active Record Migrations guide to document migration execution strategies and configurable migration backends.

*   Updated guides to clarify rate limit testing cache store configuration and to document running tests from the repository root with top-level `bin/test`.

