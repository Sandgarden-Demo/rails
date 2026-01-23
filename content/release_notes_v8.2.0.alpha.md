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
    "Active Record fixtures no longer ship a specific SQLite journal mode fixture file, reducing unused artifacts in the repository.",
    "Guides were cleaned up to remove duplicated examples and improve clarity across onboarding and encryption documentation.",
    "Active Job documentation examples were adjusted to improve copy-and-paste usability.",
    "Caching documentation was updated to cover the Rails 7.1 cache serialization format and deployment guidance.",
    "Devcontainer and generator behavior was adjusted to better support differing application names and folder mounts.",
    "Documentation and comments were standardized for clarity and to avoid unintended RDoc autolinks.",
    "Migrations guide examples were refined for correctness with PostgreSQL schemas.",
    "Test and CI infrastructure was modernized to reduce environment coupling and improve reproducibility.",
    "Dependency and lockfile maintenance reduced platform clutter and improved auditing coverage.",
    "Rails now preserves Ruby version prerelease identifiers when generating `.ruby-version`.",
    "Text rendering notes are now extracted and displayed more consistently across CSS and JavaScript contexts.",
    "Rails CI and repositories were updated to use the latest checkout action for GitHub Actions workflows.",
    "Active Support now uses the standard URI require path to reduce unnecessary specificity.",
    "Test environment handling for Ruby version manager detection was stabilized to reduce flakiness.",
    "Strict warnings configuration was relaxed for a known circular require in a queue adapter dependency.",
    "Active Storage’s GCS integration now reuses an IAM client by default while still allowing custom authorization behavior.",
    "Rails component versions and changelogs were updated as part of the 8.1.2 release preparation."
  ],
  "Breaking Changes": [
    "Rails no longer automatically loads the Sucker Punch queue adapter when Active Job starts.",
    "The Rails test stack has moved to the Minitest 6 API surface and older tooling was removed."
  ],
  "Bug Fixes": [
    "Mailer view helpers now only add CSP nonces when a nonce is actually available, preventing incorrect markup in some environments.",
    "Password helpers no longer miss required time extensions, improving correctness in environments that do not preload them.",
    "Redirect instrumentation tests no longer interfere with unrelated subscribers, improving reliability across the test suite.",
    "Log and structured event subscribers no longer assume an application root exists, preventing errors in minimal or embedded setups.",
    "Controller log subscriber tests now validate the full error message format instead of relying on an overly broad assertion.",
    "Active Record eager loading no longer produces duplicate parent rows in certain association graphs.",
    "Generated Dockerfiles now copy dependencies in a correct order so bundling works reliably in container builds.",
    "Humanization of strings now handles international alphabetic characters more accurately across locales.",
    "Custom Active Job serializer keys are now consistently available during argument serialization.",
    "Active Record structured events are now reliably emitted when expected.",
    "Runtime instrumentation now preserves whether a query result came from cache, improving observability accuracy.",
    "Time zone formatting now handles XML schema offsets correctly for locally backed DateTime values.",
    "Time JSON serialization now consistently produces UTF-8 encoded strings in standard formats.",
    "Database connection lifecycle now cleans up more safely when asynchronous exceptions occur.",
    "PostgreSQL connections now correctly reapply schema search paths after reconnecting or resetting.",
    "Schema dumping for SQLite no longer incorrectly marks some primary keys as autoincrement.",
    "Schema dumping now generates index statements using the relation’s resolved name, avoiding mismatches for renamed relations.",
    "Arel predicate exclusion is now more robust when merging equality relations with none scopes.",
    "Association preloading now groups batched loads correctly for STI hierarchies, preventing incorrect cross-class reuse.",
    "RedisCacheStore and MemCacheStore now initialize connection pools using keyword arguments, improving compatibility and correctness.",
    "Strict locals parsing now supports more realistic multiline definitions and avoids stray whitespace in extracted metadata.",
    "View form helpers now accept comma-containing values when joining arrays, preventing unexpected splitting.",
    "Delegation helpers now behave correctly in BasicObject subclasses where constants and Kernel methods can be missing.",
    "Debug exception tooling now supports editor integrations that rely on absolute source paths.",
    "Active Record exception reporting now returns the actual relation involved rather than only the class, improving debugging.",
    "Generated YAML templates now avoid accidental formatting artifacts that can confuse linters or tooling."
  ],
  "New Features": [
    "The cache store now supports an additional in-memory strategy to improve repeat access during a request.",
    "Application generation now includes a consistent test entrypoint and can be configured for different repository layouts.",
    "Application generation and CI templates now avoid creating or running system test scaffolding unless it is explicitly needed.",
    "Streaming responses can now avoid leaking selected per-request execution state into the streaming thread.",
    "Active Record enums can now use floating-point values when mapping database values to symbolic states."
  ],
  "Security": [
    "Active Storage documentation now explicitly warns about risks from user-supplied image transformations and recommends hardening the image processor configuration."
  ]
}

```
