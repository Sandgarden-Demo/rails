---
title: "Release notes v8.2.0.alpha"
date: "2026-01-23"
version: "v8.2.0.alpha"
event_time: "2026-01-23T19:55:56Z"
---

# Release notes v8.2.0.alpha

```json
{
  "Additional Changes": [
    "Active Record fixtures no longer ship a specific SQLite journal mode fixture file, reducing unused artifacts in the repository. The `journal_mode_test.sqlite3` fixture was removed from `activerecord/fixtures`.",
    "Guides were cleaned up to remove duplicated examples and improve clarity across onboarding and encryption documentation. Duplicated ERB view examples were removed from Getting Started, and Active Record Encryption docs were expanded, clarified, and had configuration options centralized into the configuring guide while also removing duplicated notes and documenting version-dependent defaults.",
    "Active Job documentation examples were adjusted so readers can use them without modifying visibility, improving copy-paste usability. The MoneySerializer example’s `klass` method is now public in the Active Job guides.",
    "Caching documentation was updated to cover the Rails 7.1 cache serialization format and deployment guidance. The guides now document the new `ActiveSupport::Cache` serialization format and how to enable and deploy it.",
    "Devcontainer and generator behavior was adjusted to better support differing app names and folder mounts. The devcontainer setup now uses a separate `app_folder` for volume mounts and includes tests for differing app name and folder name.",
    "Documentation and comments were standardized for clarity and to avoid unintended RDoc autolinks. Redirecting documentation now consistently uses “Active Support”, and occurrences of “Associations” were escaped in RDoc comments to prevent auto-linking.",
    "Migrations guide examples were refined for correctness with PostgreSQL schemas. The Active Record migrations guide now qualifies the PostgreSQL extension name with its schema in the example.",
    "Test and CI infrastructure was modernized to reduce environment coupling and improve reproducibility. Plugin/engine test commands now run under `Bundler.with_unbundled_env`, generator update tests run `bin/rails app:update` under the same isolation, and CI-specific environment variables and absolute-path handling were added to the test reporter.",
    "Dependency and lockfile maintenance reduced platform clutter and improved auditing coverage. Several gem versions were updated, musl/extra platform variants were removed from Gemfile.lock, and `bundler-audit` was updated to 0.9.3 and is always enabled in tests.",
    "Rails now preserves Ruby version prerelease identifiers when generating `.ruby-version`, improving accuracy for preview runtimes. Ruby version string generation was centralized and is covered by tests.",
    "Text rendering notes are now extracted and displayed more consistently across CSS and JS contexts. CSS note extraction was corrected, JS/CSS note test coverage was improved, and displayed note output now strips extraneous whitespace.",
    "Rails CI and repositories were updated to use the latest checkout action for GitHub Actions workflows. All workflows and Rails CI templates now use `actions/checkout@v6` instead of v5.",
    "Active Support now uses the standard URI require path to reduce unnecessary specificity. Two Active Support files were updated to require `uri` instead of `uri/generic`.",
    "Test environment handling for Ruby version manager detection was stabilized to reduce flakiness. Version manager ruby version tests now run with `RBENV_VERSION` and `rvm_ruby_string` cleared or controlled.",
    "Strict warnings configuration was relaxed for a known circular require in a queue adapter dependency. Circular require warnings for backburner are now allowed in strict warnings configuration.",
    "Active Storage’s GCS integration now reuses an IAM client by default while still allowing custom authorization behavior. The GCS IAM client is memoized with default ADC authorization and can be overridden, with tests and changelog updates.",
    "Rails component versions and changelogs were updated as part of the 8.1.2 release preparation. Versions and CHANGELOGs across components were updated, including new documented entries for Active Record, Active Support, and Railties."
  ],
  "Breaking Changes": [
    "Rails no longer automatically loads the Sucker Punch queue adapter when Active Job starts. The `SuckerPunchAdapter` autoload hook was removed from Active Job queue adapters, so applications must add an explicit require or dependency if they still use it.",
    "The Rails test stack has moved to the Minitest 6 API surface and older tooling was removed. Rails now runs on Minitest 6 with conditional compatibility for Minitest 5, and the `minitest-ci` dependency and Buildkite configuration for it were removed."
  ],
  "Bug Fixes": [
    "Mailer view helpers now only add CSP nonces when a nonce is actually available, preventing incorrect markup in some environments. `stylesheet_link_tag` now auto-adds a nonce only when `content_security_policy_nonce` is present, with a mailer regression test and a corresponding Action View changelog entry.",
    "Password helpers no longer miss required time extensions, improving correctness in environments that do not preload them. SecurePassword now requires the Active Support time core extensions explicitly, and a minor file formatting tweak was applied alongside the change.",
    "Redirect instrumentation tests no longer interfere with unrelated subscribers, improving reliability across the test suite. Redirect notification tests now unsubscribe only the notification subscriber they registered.",
    "Log and structured event subscribers no longer assume an application root exists, preventing errors in minimal or embedded setups. Exception backtrace trimming is now conditional on `Rails.root` being defined in both log subscribers and structured event subscribers.",
    "Controller log subscriber tests now validate the full error message format instead of relying on an overly broad assertion. A log assertion was tightened using a regex to match the more detailed error message output.",
    "Active Record eager loading no longer produces duplicate parent rows in certain association graphs. `eager_load` was fixed for `has_many` associations on models with composite primary keys, with a regression test and a changelog entry.",
    "Generated Dockerfiles now copy dependencies in a correct order so bundling works reliably in container builds. The generator was fixed to copy the `vendor/` directory before `bundle install`, and the base packages now include `libjemalloc2` with fixtures updated accordingly.",
    "Humanization of strings now handles international alphabetic characters more accurately across locales. `ActiveSupport::Inflector.humanize` was improved with additional tests and a changelog entry.",
    "Custom Active Job serializer keys are now consistently available during argument serialization. Serializer key handling was centralized in `ActiveJob::Serializers` and a new `:active_job_arguments` load hook ensures custom serializers are loaded for `ActiveJob::Arguments.serialize`, with tests and changelog updates.",
    "Active Record structured events are now reliably emitted when expected. The structured event subscriber is required so events load correctly, and the fix is documented in the changelog.",
    "Runtime instrumentation now preserves whether a query result came from cache, improving observability accuracy. The payload `:cached` flag is now passed into `ActiveRecord::RuntimeRegistry.record`.",
    "Time zone formatting now handles XML schema offsets correctly for locally backed DateTime values. `TimeWithZone#xmlschema` offset handling was corrected with a regression test.",
    "Time JSON serialization now consistently produces UTF-8 encoded strings in standard formats. `ActiveSupport::TimeWithZone#as_json` now returns UTF-8-encoded output when using the standard JSON time format.",
    "Database connection lifecycle now cleans up more safely when asynchronous exceptions occur. Connection configuration and state changes are wrapped in `attempt_configure_connection` to ensure clean disconnects and correct internal tracking flags.",
    "PostgreSQL connections now correctly reapply schema search paths after reconnecting or resetting. `schema_search_path` from configuration is reapplied after `reconnect!` and `reset!`, with tests and a changelog entry.",
    "Schema dumping for SQLite no longer incorrectly marks some primary keys as autoincrement. Non-autoincrement integer primary keys are preserved accurately in schema dumps, with tests and changelog updates.",
    "Schema dumping now generates index statements using the relation’s resolved name, avoiding mismatches for renamed relations. The schema dumper now inspects the result of `relation_name` when generating `add_index` statements.",
    "Arel predicate exclusion is now more robust when merging equality relations with none scopes. Where-clause predicate exclusion now uses `equality_node?`, with a regression test for merging Arel equality relations with `none`.",
    "Association preloading now groups batched loads correctly for STI hierarchies, preventing incorrect cross-class reuse. Batched association preloading now groups by both loader query and `klass`, with supporting schema and test coverage.",
    "RedisCacheStore and MemCacheStore now initialize connection pools using keyword arguments, improving compatibility and correctness. ConnectionPool initialization now uses keyword arguments (including keyword splatting where appropriate) in both stores.",
    "Strict locals parsing now supports more realistic multiline definitions and avoids stray whitespace in extracted metadata. The parsing regex now supports multiline definitions and parentheses within values, and extracted strict locals metadata now trims trailing whitespace with updated tests.",
    "View form helpers now accept comma-containing values when joining arrays, preventing unexpected splitting. `file_field` array joining now accepts values with commas, with a test and a changelog entry.",
    "Delegation helpers now behave correctly in BasicObject subclasses where constants and Kernel methods can be missing. Delegation helpers now use absolute constants and `Kernel.raise`, with tests verifying BasicObject compatibility.",
    "Debug exception tooling now supports editor integrations that rely on absolute source paths. An `absolute_path` method was added to support editor handling of syntax errors and is covered by debug exception tests.",
    "Active Record exception reporting now returns the actual relation involved rather than only the class, improving debugging. `ActiveRecord::SoleRecordExceeded#record` now returns the relation instead of the model class.",
    "Generated YAML templates now avoid accidental formatting artifacts that can confuse linters or tooling. Generator templates were updated to ensure YAML output contains no consecutive empty lines."
  ],
  "New Features": [
    "The cache store now supports an additional in-memory strategy to improve repeat access during a request. ActiveSupport::Cache::MemoryStore gained a LocalCache strategy and the related tests and changelog entries were updated accordingly.",
    "Application generation now includes a consistent test entrypoint and can be configured for different repository layouts. A top-level `bin/test` wrapper was added and the Rails test reporter can be configured via environment variables for the app root and executable path.",
    "Application generation and CI templates now avoid creating or running system test scaffolding unless it is explicitly needed. System test infrastructure files are generated on demand by the devcontainer and scaffold generators, and the default Rails CI template now includes system tests only as a commented optional step.",
    "Streaming responses can now avoid leaking selected per-request execution state into the streaming thread. ActionController::Live gained configurable exclusion of isolated execution state keys and `IsolatedExecutionState.share_with` now supports an `except:` option.",
    "Active Record enums can now use floating-point values when mapping database values to symbolic states. Enum definitions accept Float values and include schema/test coverage to validate the behavior."
  ],
  "Security": [
    "Active Storage documentation now explicitly warns about risks from user-supplied image transformations and recommends hardening the image processor configuration. The guides now include a security warning and recommend using an ImageMagick security policy when processing potentially untrusted transformations."
  ]
}

```
