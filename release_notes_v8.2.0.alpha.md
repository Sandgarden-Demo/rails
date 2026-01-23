---
title: "Release notes v8.2.0.alpha"
date: 2026-01-23
version: "8.2.0.alpha"
---

{
  "Action Pack": [
    "Applications can now merge parameter sets with custom conflict resolution while keeping strong-parameters semantics intact.",
    "Applications will now get key-aware fallback behavior when fetching missing parameters.",
    "Applications using file upload helpers will now generate standards-compliant HTML attribute values when multiple accepted types are configured.",
    "Applications using Live streaming can now exclude specific execution-state keys from being shared into streaming threads."
  ],
  "Action View": [
    "Developers using Rails tooling to find annotated notes in code will now get more accurate results across more file types.",
    "Applications using streaming rendering will now report template errors more reliably and in a way that integrates with existing error tooling.",
    "Rails’ internal parsing logic is now simpler and more consistent for templates and related tooling."
  ],
  "Active Job": [
    "Applications relying on built-in queue adapters should plan to migrate to maintained alternatives before future removals.",
    "Applications can now compute retry delays using both job context and the error that triggered the retry.",
    "Applications can once again rely on `enqueue_after_transaction_commit` behaving as a real boolean configuration with sensible defaults."
  ],
  "Active Record": [
    "Applications can now store enum values as floating-point numbers when the database column is a float.",
    "Applications using PostgreSQL connections will now retain the expected search path behavior when reconnecting or resetting connections.",
    "Applications that revert bulk table changes will now correctly undo the recorded operations against the intended table.",
    "Applications will now avoid incorrect schema output when dumping PostgreSQL schemas that already include schema-qualified identifiers.",
    "Applications performing uniqueness validations will now build comparison predicates more efficiently and with clearer intent.",
    "Applications that generate SQL via Arel tree managers can now select the correct SQL engine based on the table being operated on."
  ],
  "Active Storage": [
    "Applications can now choose whether uploads use MD5 or SHA256 checksums so they can align with their storage provider and security requirements.",
    "Applications using Google Cloud Storage URL signing can now override the IAM client while keeping a secure default."
  ],
  "Active Support": [
    "Applications will now see more consistent behavior when checking if an error has already been added.",
    "Test assertions that display callable source will now produce cleaner and more consistent output."
  ],
  "Additional Changes": [
    "Generated applications and CI templates now better match modern deployment and build expectations.",
    "Rails’ test tooling and dependencies were updated to reflect current ecosystem expectations.",
    "Developer-facing documentation and templates were clarified and corrected in multiple places."
  ],
  "Breaking Changes": [
    "Applications must run on a newer Ruby version to build and run Rails going forward.",
    "Applications should not rely on a legacy browser header being present in default responses going forward.",
    "Applications that use custom YAML loading behavior must update to newer YAML tooling expectations."
  ],
  "Bug Fixes": [
    "Applications using PostgreSQL now keep schema search path behavior consistent across connection lifecycle events.",
    "Applications using Rails’ CSRF protection will now get more predictable callback ordering when opting into prepended protection.",
    "Applications using file uploads will now see consistent HTML output for accepted MIME types when multiple values are provided.",
    "Applications using association preloading with STI models will now get correct batching behavior in more cases.",
    "Applications generating token attributes in migrations will no longer get duplicated uniqueness declarations.",
    "Applications using encrypted configuration errors will now get clearer messages when required keys are missing.",
    "Applications reverting bulk `change_table` migrations will now revert the correct table and in the correct order.",
    "Applications using PostgreSQL schema dumping with schema-qualified names will now get correct output without duplicate schema prefixes.",
    "Applications using generator and CI templates will now see fewer test failures caused by leaked environment settings.",
    "Applications with console workflows will now reliably reset executor state when reloading code.",
    "Applications using `File.atomic_write` will now get safer temporary-file creation and consistent permission handling."
  ],
  "Deprecations": [
    "Applications relying on certain built-in Active Job adapters should plan to move to externally maintained adapters.",
    "Applications using `skip_before_action :verify_authenticity_token` should plan to migrate away from skipping CSRF verification via that API."
  ],
  "Guides": [
    "The documentation now explains how to run migrations using different execution strategies so teams can tailor migration behavior to their deployment workflows.",
    "The documentation now includes clearer testing and configuration guidance for common setup issues."
  ],
  "New Features": [
    "Applications can now choose how file checksums are calculated and validated when uploading and serving files.",
    "Applications now have a shorter, clearer way to access the current Rails application object in code and examples.",
    "Applications can now read configuration values from a local .env file in development without replacing existing ENV or credentials behavior.",
    "Applications can now attach a deployment identifier that is consistently included in diagnostics and error reports.",
    "Applications can now control which execution-state keys are not shared into streaming threads when using Live streaming.",
    "Applications can now read Bearer tokens from incoming HTTP requests using a dedicated helper.",
    "Controllers can now customize how parameter hashes are merged while preserving strong-parameters behavior.",
    "Controllers can now provide more context when a fetched parameter is missing and a fallback block is used.",
    "Applications can now generate more accurate uniqueness checks without needing to acquire a database connection during query construction.",
    "Active Record can now represent enum values stored in float-backed columns.",
    "Active Job retry behavior can now adapt its wait time based on the error that occurred.",
    "Applications can now safely override how PostgreSQL custom type mappings are registered during adapter initialization."
  ],
  "Performance": [
    "Applications doing heavy whitespace normalization will now spend less time rewriting strings that do not need changes."
  ],
  "Railties": [
    "Applications can now reference the running app’s deployment revision in more places without custom wiring.",
    "Generated application templates are now more predictable in CI and local development environments.",
    "Rails’ internal parsing and developer tooling now rely on a single parsing implementation."
  ],
  "Security": [
    "Applications using request forgery protection will now have clearer guidance and safer defaults around choosing a protection strategy.",
    "Applications that disable forgery protection will no longer advertise cross-site request metadata in caching variations.",
    "Applications using header-only CSRF protection in mixed HTTP/HTTPS environments will see fewer false rejections."
  ]
}

