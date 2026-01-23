---
title: "Release notes v8.2.0.alpha"
date: 2026-01-23
version: "8.2.0.alpha"
---

{
  "Action Pack": [
    "Applications can now merge parameter sets with custom conflict resolution while keeping strong-parameters semantics intact. `ActionController::Parameters#merge` accepts a block (forwarded to `Hash#merge`) and preserves permitted status, with tests and documentation for the new behavior.",
    "Applications will now get key-aware fallback behavior when fetching missing parameters. `ActionController::Parameters#fetch` yields the requested key to the provided block when the key is missing, aligning more closely with Ruby `Hash#fetch` behavior.",
    "Applications using file upload helpers will now generate standards-compliant HTML attribute values when multiple accepted types are configured. Rails converts `file_field` `:accept` option arrays into a comma-separated string and includes regression coverage.",
    "Applications using Live streaming can now exclude specific execution-state keys from being shared into streaming threads. Rails adds `config.action_controller.live_streaming_excluded_keys`, supports exclusion in `IsolatedExecutionState.share_with`, and defers configuration until `ActionController::Live` loads via an `action_controller_live` hook."
  ],
  "Action View": [
    "Developers using Rails tooling to find annotated notes in code will now get more accurate results across more file types. `rails notes` now recognizes CSS block comments and trims whitespace from printed notes, with tests updated for CSS and JS parsing behavior.",
    "Applications using streaming rendering will now report template errors more reliably and in a way that integrates with existing error tooling. Rails routes render stream errors to `ActiveSupport.error_reporter` when configured and falls back to logging otherwise, with tests covering both the reporter and logging paths.",
    "Rails’ internal parsing logic is now simpler and more consistent for templates and related tooling. Action View removes Ripper-based parsing fallbacks and unconditionally uses Prism-based parsing where applicable."
  ],
  "Active Job": [
    "Applications relying on built-in queue adapters should plan to migrate to maintained alternatives before future removals. Rails deprecates the built-in Backburner, Sneakers, delayed_job, Resque, and queue_classic adapters with runtime deprecation warnings and tests, and updates deprecation text to indicate removal in Rails 9.0.",
    "Applications can now compute retry delays using both job context and the error that triggered the retry. `retry_on` wait procs may accept the error as an optional second argument while preserving backward compatibility based on proc arity, and documentation/tests were updated accordingly.",
    "Applications can once again rely on `enqueue_after_transaction_commit` behaving as a real boolean configuration with sensible defaults. Rails re-enables `config.active_job.enqueue_after_transaction_commit` as a working boolean and defaults it to `true` for apps loading Rails 8.2 defaults, with tests and documentation updated."
  ],
  "Active Record": [
    "Applications can now store enum values as floating-point numbers when the database column is a float. Active Record enums accept `Float` values and include coverage for float-backed enum columns.",
    "Applications using PostgreSQL connections will now retain the expected search path behavior when reconnecting or resetting connections. Active Record clears cached `schema_search_path` on new connections, re-applies it on reconnect/reset, and for PostgreSQL 18+ prefers `libpq` `parameter_status(\"search_path\")` to avoid extra `SHOW` queries.",
    "Applications that revert bulk table changes will now correctly undo the recorded operations against the intended table. Migration reversion for bulk `change_table` now uses the provided `table_name` and replays recorded operations in reverse, with a regression test for prefixed table names.",
    "Applications will now avoid incorrect schema output when dumping PostgreSQL schemas that already include schema-qualified identifiers. PostgreSQL schema dumping no longer double-prefixes schema names for already schema-qualified relation names, with tests and a documented changelog entry.",
    "Applications performing uniqueness validations will now build comparison predicates more efficiently and with clearer intent. Arel adds case-sensitive and case-insensitive equality operators that are used by uniqueness validation query building to avoid checking out a database connection during query construction.",
    "Applications that generate SQL via Arel tree managers can now select the correct SQL engine based on the table being operated on. TreeManager now derives the SQL engine from the associated table’s klass (instead of always using `Table.engine`), and tests verify behavior for delete/insert/update managers."
  ],
  "Active Storage": [
    "Applications can now choose whether uploads use MD5 or SHA256 checksums so they can align with their storage provider and security requirements. Active Storage supports configurable checksum algorithms for services and direct uploads, centralizing checksum computation and verification in `ActiveStorage::Service`.",
    "Applications using Google Cloud Storage URL signing can now override the IAM client while keeping a secure default. Active Storage restores and memoizes an overridable IAM client that defaults to Application Default Credentials (ADC) for GCS URL signing, with tests verifying both default and overridden credential behavior."
  ],
  "Active Support": [
    "Applications will now see more consistent behavior when checking if an error has already been added. `Errors#added?` now ignores `:callback` and `:message` options symmetrically between stored errors and query options, with tests covering the corrected option handling.",
    "Test assertions that display callable source will now produce cleaner and more consistent output. Rails strips leading stabby-lambda syntax (`-\u003e`) from callable source strings before further processing in ActiveSupport test assertions."
  ],
  "Additional Changes": [
    "Generated applications and CI templates now better match modern deployment and build expectations. Rails conditionally includes `libvips` packages for Active Storage-enabled apps, makes system tests an optional commented-out CI step, and standardizes Ruby version selection across templates and devcontainer defaults (including Ruby 4.0.0 availability).",
    "Rails’ test tooling and dependencies were updated to reflect current ecosystem expectations. Rails upgrades and adapts integration for Minitest 5/6, adjusts test runners and helpers, removes some unused test dependencies, and clarifies documentation for running tests from the repository root using top-level `bin/test`.",
    "Developer-facing documentation and templates were clarified and corrected in multiple places. Rails updates various guide examples (including `email_address` usage, Active Record finder examples, and configuration snippets), clarifies `humanize` behavior with `_id` suffixes and date helper semantics, and refreshes links and issue templates."
  ],
  "Breaking Changes": [
    "Applications must run on a newer Ruby version to build and run Rails going forward. Rails raises the minimum supported Ruby version from 3.2 to 3.3 and removes compatibility code paths that existed only for Ruby versions older than 3.3.",
    "Applications should not rely on a legacy browser header being present in default responses going forward. Rails removes the `X-XSS-Protection` header from the Rails 8.2 default HTTP response headers and updates templates and documentation to match.",
    "Applications that use custom YAML loading behavior must update to newer YAML tooling expectations. Rails now requires Psych \u003e= 4 and replaces conditional fallbacks to `YAML.load` with `YAML.unsafe_load` / `YAML.unsafe_load_file` throughout core code and tests."
  ],
  "Bug Fixes": [
    "Applications using PostgreSQL now keep schema search path behavior consistent across connection lifecycle events. Rails clears the `schema_search_path` cache on new connections and re-applies it on reconnect/reset, and for PostgreSQL 18+ it prefers `libpq` `parameter_status(\"search_path\")` to avoid issuing `SHOW search_path` when possible.",
    "Applications using Rails’ CSRF protection will now get more predictable callback ordering when opting into prepended protection. `protect_from_forgery prepend: true` now prepends both forgery protection callbacks in the correct order, and the test suite asserts the exact callback ordering.",
    "Applications using file uploads will now see consistent HTML output for accepted MIME types when multiple values are provided. Rails normalizes `file_field` `:accept` option arrays into a comma-separated attribute value and includes a regression test for the behavior.",
    "Applications using association preloading with STI models will now get correct batching behavior in more cases. Batched association preloading now groups by both `loader_query` and `klass`, with STI-based test models, schema adjustments, and serialization changes verifying correctness.",
    "Applications generating token attributes in migrations will no longer get duplicated uniqueness declarations. Token attribute generation now emits `unique: true` only once and centralizes uniqueness handling in `inject_index_options`.",
    "Applications using encrypted configuration errors will now get clearer messages when required keys are missing. `EncryptedConfiguration#require` now raises `KeyError` with a descriptive \"Missing key: ...\" message (including nested keys), and tests assert on the exact message.",
    "Applications reverting bulk `change_table` migrations will now revert the correct table and in the correct order. Bulk `change_table` reversion now uses the provided `table_name` and replays recorded operations in reverse order, with regression tests for prefixed table names.",
    "Applications using PostgreSQL schema dumping with schema-qualified names will now get correct output without duplicate schema prefixes. Rails prevents double-prefixing for already schema-qualified relation names during PostgreSQL schema dumps and adds tests and a changelog entry for the fix.",
    "Applications using generator and CI templates will now see fewer test failures caused by leaked environment settings. Rails scopes environment variables like `DATABASE_URL`, `RAILS_ENV`, `RAILS_RELATIVE_URL_ROOT`, `RACK_ENV`, and `FROM` to individual tests/tasks via helper scoping, and adds ENV leak detection to improve isolation.",
    "Applications with console workflows will now reliably reset executor state when reloading code. Rails console `reload!` now resets the application executor when it is active, with tests and a documented changelog entry.",
    "Applications using `File.atomic_write` will now get safer temporary-file creation and consistent permission handling. `File.atomic_write` now uses an exclusive, random temporary filename (SecureRandom/hex suffix) and always preserves existing file permissions, with tests updated to assert uniqueness, format, and permission behavior."
  ],
  "Deprecations": [
    "Applications relying on certain built-in Active Job adapters should plan to move to externally maintained adapters. Rails deprecates the built-in Backburner, Sneakers, delayed_job, Resque, and queue_classic adapters with runtime deprecation warnings and tests, and deprecation messages were updated to target removal in Rails 9.0.",
    "Applications using `skip_before_action :verify_authenticity_token` should plan to migrate away from skipping CSRF verification via that API. Rails routes this skip through a no-op flagging callback, gates CSRF verification based on that tracked execution, and emits a deprecation warning when the callback is skipped while preserving request handling."
  ],
  "Guides": [
    "The documentation now explains how to run migrations using different execution strategies so teams can tailor migration behavior to their deployment workflows. The Active Record Migrations guide documents migration execution strategies and describes swappable migration backends with configuration examples.",
    "The documentation now includes clearer testing and configuration guidance for common setup issues. Guides clarify rate limit testing cache store configuration, document running tests with top-level `bin/test` from the repository root, and update examples and links across multiple guides."
  ],
  "New Features": [
    "Applications can now choose how file checksums are calculated and validated when uploading and serving files. Active Storage now supports configurable checksum algorithms (MD5 or SHA256) for services and direct uploads, with checksum computation and verification centralized in `ActiveStorage::Service`.",
    "Applications now have a shorter, clearer way to access the current Rails application object in code and examples. Rails adds `Rails.app` as an alias for `Rails.application` and updates internal and guide examples to prefer `Rails.app`, including common credentials access patterns.",
    "Applications can now read configuration values from a local .env file in development without replacing existing ENV or credentials behavior. Rails adds a `.env`-backed backend exposed as `Rails.app.dotenvs` and wires it into `Rails.app.creds` resolution in development (between `ENV` and encrypted credentials), and new applications generate a default `.env` file.",
    "Applications can now attach a deployment identifier that is consistently included in diagnostics and error reports. Rails adds `Rails.app.revision` / `config.revision`, exposes it in `Rails::Info` and Rails error context, and later restricts `config.revision` to string values (removing Proc support) while improving the git-based fallback implementation.",
    "Applications can now control which execution-state keys are not shared into streaming threads when using Live streaming. Rails introduces `config.action_controller.live_streaming_excluded_keys`, extends `IsolatedExecutionState.share_with` to support excluding keys, and defers Live configuration via an `action_controller_live` load hook.",
    "Applications can now read Bearer tokens from incoming HTTP requests using a dedicated helper. Rails adds `ActionDispatch::Request#bearer_token` to parse the `Authorization: Bearer ...` header and return the token (or `nil`) in a consistent way.",
    "Controllers can now customize how parameter hashes are merged while preserving strong-parameters behavior. `ActionController::Parameters#merge` now accepts a block (forwarded to `Hash#merge`) while maintaining permitted/unpermitted status and includes tests and documentation for the new behavior.",
    "Controllers can now provide more context when a fetched parameter is missing and a fallback block is used. `ActionController::Parameters#fetch` now yields the requested key to the provided block when the key is absent, matching Ruby `Hash#fetch` semantics more closely.",
    "Applications can now generate more accurate uniqueness checks without needing to acquire a database connection during query construction. Arel adds case-sensitive and case-insensitive equality operators that are used by uniqueness validation query building to avoid connection-backed type lookups.",
    "Active Record can now represent enum values stored in float-backed columns. Enums now allow `Float` values and include tests that verify behavior when the underlying database column type is a float.",
    "Active Job retry behavior can now adapt its wait time based on the error that occurred. `retry_on` wait procs may now accept the raised error as an optional second argument (with arity-based backward compatibility), and the behavior is covered by updated tests and documentation.",
    "Applications can now safely override how PostgreSQL custom type mappings are registered during adapter initialization. The PostgreSQL adapter adds a class-level callback API to register and run custom type-mapping callbacks during type map initialization, and explicitly initializes the callback storage before use."
  ],
  "Performance": [
    "Applications doing heavy whitespace normalization will now spend less time rewriting strings that do not need changes. `String#squish!` uses a more selective whitespace-collapsing regular expression to avoid replacing single ASCII spaces while still normalizing other whitespace to one space."
  ],
  "Railties": [
    "Applications can now reference the running app’s deployment revision in more places without custom wiring. Railties exposes `Rails.app.revision` / `config.revision` in `Rails::Info` and ensures Rails error context includes the revision and environment, with documentation and tests covering the behavior.",
    "Generated application templates are now more predictable in CI and local development environments. Railties standardizes Ruby version selection across templates, makes system tests opt-in in CI templates, and updates devcontainer defaults to include Ruby 4.0.0 among selectable versions.",
    "Rails’ internal parsing and developer tooling now rely on a single parsing implementation. Railties removes Ripper-based parsing fallbacks and unconditionally uses Prism for render/notes/test parsing paths."
  ],
  "Security": [
    "Applications using request forgery protection will now have clearer guidance and safer defaults around choosing a protection strategy. Calling `protect_from_forgery` without `:with` is deprecated, a configurable default strategy is introduced, and Rails 8.2 defaults set the strategy to `:exception`.",
    "Applications that disable forgery protection will no longer advertise cross-site request metadata in caching variations. When forgery protection is skipped, Rails also skips adding `Sec-Fetch-Site` to the `Vary` header (including inherited protection), aligning header behavior with the disabled CSRF checks.",
    "Applications using header-only CSRF protection in mixed HTTP/HTTPS environments will see fewer false rejections. Rails allows missing `Sec-Fetch-Site` on HTTP when SSL is not forced and refactors header verification logic to a clearer case-based implementation with updated tests."
  ]
}

