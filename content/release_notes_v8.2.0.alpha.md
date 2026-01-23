---
title: "Release notes v8.2.0.alpha"
date: "2026-01-23"
version: "v8.2.0.alpha"
event_time: "2026-01-23T19:55:56Z"
---

# Release notes v8.2.0.alpha

This release includes changes merged through `2026-01-23T19:55:56Z`.

## Active Support

*   Added a per-request `LocalCache` strategy to
    `ActiveSupport::Cache::MemoryStore` to reduce repeat cache lookups.
*   Updated `ActiveSupport::Inflector.humanize` to handle international
    alphabetic characters more accurately.
*   Fixed `ActiveSupport::TimeWithZone#xmlschema` offset handling for locally
    backed `DateTime` values.
*   Fixed `ActiveSupport::TimeWithZone#as_json` to return UTF-8 encoded output
    when using the standard JSON time format.
*   Updated `RedisCacheStore` and `MemCacheStore` to initialize connection pools
    using keyword arguments.
*   Updated delegation helpers to use absolute constants and `Kernel.raise` for
    improved `BasicObject` compatibility.
*   Updated Active Support to require `uri` instead of `uri/generic` where
    applicable.

## Active Model

*   Fixed `ActiveModel::SecurePassword` to require the Active Support time core
    extensions explicitly.

## Active Record

*   Removed an unused SQLite journal mode fixture file from the Active Record
    fixtures.
*   Added support for Float values in enum definitions.
*   Fixed `eager_load` for `has_many` associations on models with composite
    primary keys to avoid duplicate parent rows.
*   Fixed emission of Active Record structured events by ensuring the structured
    event subscriber is required.
*   Updated runtime instrumentation to record the `:cached` payload flag in
    `ActiveRecord::RuntimeRegistry`.
*   Updated connection configuration to wrap state changes in
    `attempt_configure_connection` for safer disconnects on asynchronous
    exceptions.
*   Fixed PostgreSQL adapters to reapply `schema_search_path` from configuration
    after `reconnect!` and `reset!`.
*   Fixed SQLite schema dumping to avoid incorrectly marking non-autoincrement
    integer primary keys as `AUTOINCREMENT`.
*   Updated schema dumping to generate `add_index` statements using the resolved
    relation name.
*   Fixed predicate exclusion when merging Arel equality relations with `none`
    scopes.
*   Fixed batched association preloading for STI hierarchies by grouping loads
    by loader query and `klass`.
*   Updated `ActiveRecord::SoleRecordExceeded#record` to return the relation
    involved rather than only the model class.

## Action View

*   Fixed `stylesheet_link_tag` to add a CSP nonce only when
    `content_security_policy_nonce` is available.
*   Fixed strict locals parsing to support multiline definitions and trim
    trailing whitespace in extracted metadata.
*   Fixed `file_field` to accept comma-containing values when joining arrays.

## Action Pack

*   Added an `except:` option to `IsolatedExecutionState.share_with` and
    configurable exclusion of isolated execution state keys for
    `ActionController::Live` streaming.
*   Fixed redirect notification tests to unsubscribe only the subscriber they
    register.
*   Fixed controller log subscriber assertions to validate the full error
    message format.
*   Added `absolute_path` support in debug exception tooling for editor
    integrations requiring absolute source paths.

## Active Job

*   Removed automatic loading of the Sucker Punch queue adapter, requiring an
    explicit dependency or `require` when used.
*   Fixed custom serializer loading by centralizing serializer key handling and
    ensuring custom serializers are loaded for argument serialization.
*   Updated strict warnings configuration to allow circular require warnings for
    the `backburner` dependency.

## Action Mailer

*   No changes.

## Action Cable

*   No changes.

## Active Storage

*   Updated the GCS service integration to memoize the IAM client by default
    while allowing overrides.
*   Updated Active Storage documentation to warn about risks from user-supplied
    image transformations and recommend an ImageMagick security policy.

## Action Mailbox

*   No changes.

## Action Text

*   No changes.

## Railties

*   Updated Rails to run on the Minitest 6 API surface and removed the
    `minitest-ci` dependency and related Buildkite configuration.
*   Fixed generated Dockerfiles to copy `vendor/` before running
    `bundle install` and added `libjemalloc2` to base packages.
*   Added a top-level `bin/test` wrapper and configurable test reporter
    environment variables for application generation.
*   Updated generators and CI templates to create and run system test scaffolding
    only when required.
*   Updated the devcontainer setup to support separate application folder volume
    mounts and differing application and folder names.
*   Updated plugin and engine test commands and generator update tests to run
    under `Bundler.with_unbundled_env` for improved isolation.
*   Updated gem dependencies, reduced `Gemfile.lock` platform variants, and
    updated `bundler-audit` to 0.9.3 with always-on test enforcement.
*   Updated Ruby version string generation to preserve prerelease identifiers
    when generating `.ruby-version`.
*   Updated CI workflows and templates to use `actions/checkout@v6`.
*   Updated environment handling in Ruby version manager detection tests to
    reduce flakiness.
*   Updated YAML generator templates to avoid consecutive empty lines.
*   Fixed log and structured event subscribers to avoid assuming `Rails.root` is
    defined.
*   Updated component versions and changelogs as part of the 8.1.2 release
    preparation.

## Guides

*   Updated the Getting Started guide to remove duplicated ERB view examples and
    refined the Active Record Encryption documentation.
*   Updated Active Job guide examples to keep serializer methods public for
    improved example usability.
*   Updated caching guides to document the Rails 7.1 cache serialization format
    and deployment guidance.
*   Updated the migrations guide to qualify PostgreSQL extension names with
    their schema in examples.
*   Updated text note extraction to display notes consistently across CSS and
    JavaScript contexts.
*   Updated redirecting documentation terminology and escaped "Associations" in
    RDoc comments to avoid unintended autolinks.
