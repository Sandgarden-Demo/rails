# Documentation Style Guide

## Context

**Project:** Doc Holiday - AI-powered documentation and release notes generator  
**Runtime:** Go backend with Next.js frontend  
**Core Technology:** Hugo static site generator, Hextra theme, AI content generation  
**Publishing System:** Hugo with Hextra theme (Tailwind CSS)

## Primary Documentation Goals

Generate documentation that enables Doc Holiday customers to:
1. Deploy and configure Doc Holiday for their projects
2. Connect source repositories and configure publications
3. Generate automated documentation and release notes
4. Understand configuration options and style guides
5. Troubleshoot issues using run logs

IMPORTANT: this is an public/end-user facing documentation, YOU MUST be careful with not expose internal jargon.

## Writing Rules

### Core Principles
- **Be concise** - Use minimum words necessary
- **Be practical** - Focus on actionable information
- **Be example-driven** - Show working code for every concept
- **Be customer-focused** - Document user-visible changes only

### Internal vs Changes - rulebook

Changes done in the following packages are ALWAYS INTERNAL CHANGES, their domain is `internal`: 
- ./js/packages
- ./js/apps/sfs
- ./js/apps/doc.holiday/e2e
- ./js/apps/doc.holiday/node_modules
- ./js/apps/doc.holiday/scripts
- ./sfs/clerk 
- ./sfs/e2e 
- ./sfs/cmd 
- ./sfs/docker 
- ./sfs/internal/aikit 
- ./sfs/internal/cmd 
- ./sfs/internal/crypto 
- ./sfs/internal/dao 
- ./sfs/internal/env 
- ./sfs/internal/jobmgr 
- ./sfs/internal/otelutil 
- ./sfs/internal/secrets 
- ./sfs/internal/textsearch 
- ./sfs/middleware 
- ./sfs/sfsclient 
- ./vendor 
- ./go.mod 
- ./go.sum

YOU MUST NOT document changes to e2e test suites (`./js/apps/doc.holiday/e2e` and `./sfs/e2e`), you are ALLOWED to use the changes from e2e tests to document customer facing changes.

In `sfs/sfs/generated_serve_*.go` files, you are going to get the entry point to API changes. BE CAREFUL: not all API changes are public-facing.

When reporting data modeling changes, you have to make sure that internal names are not exposed untranslated to the public facing. Leaking internals is SEVERE MISTAKE.

Do:
- models.JobToResponse -> response message to user initiated jobs

Don't
- models.JobToResponse -> `models.JobToResponse`
- models.JobToResponse -> `JobToResponse`
- models.JobToResponse -> `jobToResponse`

### Instructional Documentation Guidance - What is an external feature? What is an user-facing functionality?
As you read the source code to compose documentation changes, you are going to see modifications that may or may not be user-facing. Your biased should be to error on the side of thinking a change is an internal change.

YOU MUST READ the section `Internal vs Changes - rulebook`

YOU MUST APPLY THIS STANDARD THOUGH:
1. If a change modifies javascript files in the frontend, and you see these modifications are changing the layout or the content of a page or page fragment, it may be a user-facing change.
2. If a change happens in the backend source code, it is very likely it is an internal change.
3. If a change happens in a constant in the backend code, it may be change a certain parameter of the system that might need to be reported as user-facing; only report the change if you are absolutely sure it is user-facing.
4. Business logic changes are good candidates to be user-facing changes, when they do happen though, you MUST FOLLOW the instructions in `Tone Guidelines` and `Non-Technical User Adjustments`.
5. Data Modeling changes are ALWAYS internal changes. In this scenario, you need to find the equivalent change in the frontend code, and decide if they are visible by the user, and then report THAT instead.

IMPORTANT: never reference internal git commit SHAs or internal issues numbers.

### Release Notes Guidance - What is an external feature? What is an user-facing functionality?
As you read the source code to compose documentation changes, you are going to see modifications that may or may not be user-facing. Your biased should be to error on the side of thinking a change is an internal change.

YOU MUST READ the section `Internal vs Changes - rulebook`

YOU MUST APPLY THIS STANDARD THOUGH:
1. For internal changes, report the name of the subsystem that has been changed and whether it is an improvement, a fix, our security fix. You MUST NOT explain the change. For example: `Updated AI inference logging system for improved service to final user`.
2. For potentially user-facing changes, apply the same standard as internal changes. For example: `Updated AI inference logging system for improved service to final user`.
3. For user-facing changes (the ones that you can safely confirm they would be visible in the frontend application, or by that a certain user usable capability was now made available), you MUST FOLLOW the instructions in `Tone Guidelines` and `Non-Technical User Adjustments`.
4. Data Modeling changes are ALWAYS internal changes. In this scenario, you need to find the equivalent change in the frontend code, and decide if they are visible by the user, and then report THAT instead.

IMPORTANT: never reference internal git commit SHAs or internal issues numbers.

### Tone Guidelines

#### Default Tone (Technical Users)
- Clear, direct documentation language
- Assume familiarity with Git, CI/CD, documentation tools
- Use technical terminology appropriately
- Focus on configuration examples
- Avoid internal implementation details

#### Non-Technical User Adjustments
When writing for non-technical Doc Holiday users:
- Explain documentation generation concepts
- Provide UI navigation guidance
- Include screenshots for complex workflows
- Link to Hugo and Hextra documentation
- Offer troubleshooting for common issues

### Content Structure Rules

#### File Requirements
1. **Format:** Markdown (.md) in `/content/` directory only
2. **Hugo front matter (REQUIRED for documentation):**
```yaml
---
title: "Page Title"
description: "Brief description of the content"
type: docs
weight: 10
---
```
3. **Release notes front matter (SPECIFIC):**
```yaml
---
title: Release Notes
description: Brief description of the thing
type: docs
weight: 7
---
```
4. **File locations:** ALL files in `/content/` directory or subdirectories

#### Page Structure - Documentation
1. Never use H1 (#)
2. Overview paragraph
3. Prerequisites (if needed)
4. Step-by-step instructions
5. Code examples with syntax highlighting
6. Troubleshooting tips
7. Related links

#### Page Structure - Release Notes (SPECIAL FORMAT)
1. **Location:** ONLY `/content/release-notes.md`
2. **Structure:** Add new entries at TOP of file
3. **Date format:** `## YYYY-MM-DD` (current date)
4. **Categories with emojis:**
   - `### 🚀 New Features`
   - `### ✨ Enhancements`
   - `### 🐛 Bug Fixes`
   - `### 💼 Known Issues`
6. **Entry format:**
```markdown
- **Feature Name**
  - Description of the feature or fix
```
6. **NEVER alter older entries**
7. **One change per entry (no combining)**

#### Heading Rules
- **Never use H1 (#)**
- **H2 (##)** - Major sections or dates (release notes)
- **H3 (###)** - Subsections or categories (with emojis for release notes)
- **H4+ (####)** - Rarely used for details
- **Consistent formatting with existing docs**

#### Directory Placement Rules
- **`/content/_index.md`** - Landing page overview
- **`/content/getting-started.md`** - Deployment and UI navigation
- **`/content/sources.md`** - Source connections (GitHub, JIRA, Notion)
- **`/content/publications.md`** - Mapping sources to documentation
- **`/content/run-logs.md`** - Run logs UI documentation
- **`/content/release-notes.md`** - Chronological release notes
- **`/content/interactions.md`** - Instructions and examples on how the human partner can use Doc Holiday to write documentation
- **`/content/style-guide.md`** - Explains the basic structures and provide examples of productive and effective style guides that customers can use
- **NEVER create files outside `/content/`**

### Formatting Requirements

#### Lists
- Use bullets for unordered lists
- No periods at end of list items
- Use Oxford comma in series
- Indent nested items properly

#### Release Notes Specific
- **Emojis required** for category headers
- **Bold** for feature names
- Indented descriptions with `-`
- Chronological order (newest first)
- Current date for new entries

#### Code Examples
- Always use syntax highlighting
- Include comments for complex logic
- Show input and expected output
- Use appropriate language tags

### Code Example Requirements

1. **Language tags:**
   - `yaml` for configuration
   - `bash` or `shell` for commands
   - `javascript`/`typescript` for frontend
   - `go` for backend examples
2. **Configuration examples must include:**
   - Complete YAML structure
   - Comments explaining options
   - Default values noted
3. **Show realistic examples**
4. **Include error cases**
5. **Provide troubleshooting context**

### Linking Rules

#### Internal Links
- **Hugo format:** `[Page Title](/page-slug/)`
- **Anchor links:** `[Section](#section-name)`
- **Related docs:** Link between related features
- Reference Hugo docs: `https://gohugo.io/documentation/`
- Reference Hextra docs: `https://imfing.github.io/hextra/docs/`

#### External Links
- Full URLs for external resources
- Hugo documentation for framework details
- Hextra documentation for theme features
- GitHub for source examples

### DO's and DON'Ts

#### DO - Documentation:
- Create files only in `/content/` directory
- Use Hugo front matter with weight values
- Follow Hextra theme conventions
- Link to comparable Hextra examples
- Include UI navigation guidance
- Document customer-visible features
- Provide configuration examples
- Add troubleshooting sections

#### DON'T - Documentation:
- Create files outside `/content/`
- Document internal implementation
- Include CI/CD pipeline details
- Mix release notes with documentation
- Document test changes
- Skip front matter metadata
- Use complex technical jargon unnecessarily
- Rewrite existing writing simply to change the style

#### DO - Release Notes:
- Add new entries at the TOP
- Use current date
- Use emoji categories (🚀 🐛 ✨ 💼)
- Bold feature names
- Keep descriptions concise
- One change per entry
- Focus on user impact
- Maintain chronological order

#### DON'T - Release Notes:
- Alter existing entries
- Combine multiple changes
- Skip the emoji format
- Include internal changes
- Document CI/CD updates
- Mix with regular documentation
- Create new categories
- Change the file structure
- Delete periods 

## Documentation Content Examples
- Below are 3 examples of existing documentation that you should use for reference, including formatting, structure, layout, style, and language.
- The start and end of the following examples is marked by 10 dashes in a row, like this ----------. The 10 dashes in a row are not part of the formatting or content of the examples.

### Documentation Page Example 1 - This explains the supported ways of interacting with Doc Holiday with specific commands and actions
----------

---
title: Interacting with Doc Holiday
description: Brief description of the thing
type: docs
weight: 2
---

Doc Holiday automatically runs based on existing workflows and patterns, such as cutting a release, merging code, or the completion of a workflow run.  You can also interact with it directly via a chat interface.  Doc Holiday will respond to `@doc.holiday` or `@sandgarden` within an existing issue (issue comments), new issue, pull requests, or releases.  The good doctor understands the following categories of commands:

> **Note:** If the publication input repo is different from the target repo, references to time periods must be made within issues or issue comments in the input repo itself.

## Creating Release Notes
Doc Holiday will create a pull request containing release notes in response to newly created issues, comments on existing issues, comments on existing pull requests or merge requests, and when triggered by releases, workflow runs, or merges.

- "@doc.holiday write release notes about the new feature developed in /mono/analysis/time_series/"
- "@doc.holiday create release notes covering the last 20 commits"
- "@doc.holiday make release notes addressing all commits from 2a364710beefcff2bbd6c230a54596932141101a to 119210238c548e4bc97fa053e56292edf8e45299"
- "@doc.holiday generate release notes for changes over the last two weeks"
   
## Creating New Documentation
Doc Holiday will create a pull request (or merge request) containing new documentation in response to newly created issues, comments on existing issues, and comments on existing pull requests or merge requests, as well as triggers from new releases, workflow runs, and merges.

- "@doc.holiday create new documentation about the new feature developed in /mono/analysis/time_series/"
- "@doc.holiday add a document to this PR about how the new methods of authentication work"
- "@doc.holiday make new documentation about functionality contained in the last week or code changes"
  - Please note: if the input (source) repo is different from the target repo, references to time periods must be made on issues or issue comments in the input repo itself.

## Updating Existing Documentation
Doc Holiday will create a pull request (or merge request) containing updates to existing documentation in response to newly created issues, comments on existing issues, comments on existing pull requests or merge requests, and comments in a PR or MR review. Requests can also be triggered by new merges or workflow runs.

- "@doc.holiday update the documentation about supported databases to include our new support for Postgres"
- "@doc.holiday rewrite the contents of this PR in the voice of a product marketing manager who is speaking to a non-technical audience. Be concise and provide benefit statements"
- "@doc.holiday update the documentation on account setup to reflect changes in the last 20 commits"
- "@doc.holiday revise the /mono/docs/getting_started.md document to exclude any mention of configuring Kubernetes" 
- "@doc.holiday replace all mentions of "connections" with "integrations" throughout all the documentation"
- "@doc.holiday please revert the most recent change"
  - PR or MR comments only
- "@doc.holiday delete the /mono/docs/deprecated.md file"
- "@doc.holiday move the /mono/drafts/user.md file to the /mono/docs/ directory"

----------

### Documentation Page Example 2 - This explains how to create and configure publications in Doc Holiday
----------

---
title: Publications
description: Brief description of the thing
type: docs
weight: 4
---
Publications define which sources Doc Holiday will read from, where it will write to, what kinds of content it will write, and what kinds of activity will trigger it.

### Configuration Options
* Name - this will serve as an internal reference within Doc Holiday
* Inputs - connections that contain information used for generating words
* Targets - repositories that Doc Holiday will write documentation and release notes to
* React to: - define what type of activity, in both source and destination repos, Doc Holiday should react to
  * Pull Requests - creating a new pull request with a description containing `@sandgarden` or `@doc.holiday`, followed by an instructive prompt
  * New Issues - creating a new issue with a description containing `@sandgarden` or `@doc.holiday`, followed by an instructive prompt
  * Issue Comments - adding a comment to an existing issue containing `@sandgarden` or `@doc.holiday`, followed by an instructive prompt. This currently honors only  comments on issues within the source repos.
  * Releases - creating new releases within source repos
  * Workflow Runs - triggering workflow runs in a connected GitHub repository based on the outcome of a specific workflow.
      1. Path to workflow file - specify the workflow path to monitor (e.g., `/github/workflows/deploy.yml`). You may also specify the workflow trigger (ID) for greater control over automation.
      2. When the workflow completes, Doc Holiday will generate the appropriate content based on configuration.
      - **Supported Events:** GitHub Workflow Run events are supported for connected repositories and triggerable via the workflow's unique path or ID.
* Write: - define what types of writing Doc Holiday should draft
  * Release Notes - will create lists of changes based upon the source repos defined and referenced in the triggering activity
  * Documentation - will update existing and create new documentation, as required, based upon the current documentation in the repo
* Style Guide URL (optional, but strongly recommended) - link to a public URL or Google Drive doc for use as a style guide with this specific publication.

----------

### Documentation Page Example 3 - This explains all of the supported types of sources for creating Connections in Doc Holiday
----------

---
title: Connections
description: Brief description of the thing
type: docs
weight: 3
---
Connections contain information that Doc Holiday leverages when creating documentation and release notes.

Doc Holiday analyzes data from your connected repositories, including historical commit information and prior changes. When generating documentation or release notes (such as when responding to ambiguous requests or major file updates), Doc Holiday attempts to retrieve relevant context from previous work and integrates commit summaries, messages, and diffs, where available, for more accurate and comprehensive documentation.

# Currently Supported
## GitHub
GitHub repositories are supported as both sources and destinations of information.  The repository URL provided must be accessible to the GitHub application deployed within your organization. Branches can be specified for Doc Holiday-generated pull requests to write to; if the field is left empty, it will default to 'main'.

GitHub connections fully support automation triggers for workflow run events (GitHub Actions). When creating a GitHub connection, Doc Holiday will automatically subscribe to workflow run webhook events, enabling you to trigger documentation and release note generation automatically when workflows complete in your repository. When configuring Publications, you can select specific workflow files to monitor as automation triggers.

To create a GitHub connection:
* Go to the Connections screen (https://app.doc.holiday/connections), click the 'Add Connection' button in the upper right corner, and select 'GitHub' from the Provider dropdown.
* Provide a recognizable name and select which GitHub repository you want to make the connection for from the dropdown menu. It will automatically be populated with all repositories that the GitHub application was given permission to access at installation.

> **Note:** Only repositories accessible via your Doc Holiday application (up to 100 repositories per organization) will appear in the selection list. If you have more than 100 repositories, only the first 100 accessible repos will be shown.

## GitLab
GitLab repositories are supported as both sources and destinations of information.  The repository URL provided must be accessible to the GitLab application deployed within your organization. Branches can be specified for Doc Holiday-generated pull requests to write to; if the field is left empty, it will default to 'main'.

To create a GitLab connection:
* In GitLab, [create a project access token](https://docs.gitlab.com/user/project/settings/project_access_tokens/) and save the token generated for later use.
* Go to the Connections screen (https://app.doc.holiday/connections), click the 'Add Connection' button in the upper right corner, and select 'GitLab' from the Provider dropdown.
* Give the connection a recognizable name, enter the name of the GitLab project, and paste in the access token you copied previously.

## Google Drive
Google Drive is supported as a source of information, as well as a way to host style guides used (either globally or on a per-Publication basis).  Authentication is created using service account credentials.

To connect Google Drive:
* Ensure the Google Drive API is enabled within the Google Cloud console.
* Create a service account.  It can be fairly low-permissioned (e.g. a basic viewer or editor role).
* Once the service account has been created, please create a key for it. The key will generate a downloadable JSON file containing the credentials.
* Go to the Connections screen (https://app.doc.holiday/connections), click the 'Add Connection' button in the upper right corner, and select 'Google Drive' from the Provider dropdown.
* Copy and paste the contents of the downloaded JSON file, or upload the file into the field.
  
> **Note:** When the service account above was created, it generated a email-y name, like doc-holiday@laptop-llm.iam.gserviceaccount.com.  Please make sure any documents Doc Holiday needs to read are shared with that username / address.

## Confluence
Confluence is supported as a source of documentation and wiki content for Doc Holiday. Content from your connected Confluence workspaces can be referenced for context during documentation and release note generation. 

To connect Confluence:
* In Confluence, create or use an existing account with the appropriate API access.
* Generate or retrieve a Confluence API token.
* Go to the Connections screen (https://app.doc.holiday/connections), click the 'Add Connection' button in the upper right corner, and select 'Confluence' from the Provider dropdown.
* Enter a name for your Confluence connection, the Confluence **site URL** (e.g., `yoursubdomain.atlassian.net/wiki`), and required credentials or API token.

> **Note:** Only spaces, pages, and content accessible via the provided credentials can be indexed by Doc Holiday.

## Notion
Notion is supported as a source of context for Doc Holiday. Product specs, design docs, and any other supplementary content shared from your connected Notion workspaces is used in the indexing, context building, and content generation process.

To connect Notion:
* In Notion, [create an integration](https://developers.notion.com/docs/authorization) and grab the integration key generated for use in Doc Holiday.
* Go to the Connections screen (https://app.doc.holiday/connections), click the 'Add Connection' button in the upper right corner, and select 'Notion' from the Provider dropdown.
* Enter a name for your Notion connection and paste the integration key copied previously.

## External Documentation
The External Documentation connection type allows you to connect a public documentation repository to Doc Holiday. This provides additional reference context for generating high-quality release notes and for updating or synthesizing new documentation. Doc Holiday will crawl and index the contents of the provided documentation site, making it available for AI-assisted generation and updates.

To connect a Documentation Site:
* Go to the Connections screen (https://app.doc.holiday/connections), click the 'Add Connection' button in the upper right corner, and select 'External Documentation' from the Provider dropdown.
* Enter a name for your connection and the public URL of the root documentation site (e.g., your Docusaurus or Hugo docs deployment).

> **Note:** Only publicly accessible HTML sites are supported for initial releases. Access to sites behind authentication or paywalls is not currently supported.

### *More coming soon!*

----------

### Release Notes Example
----------

---
title: Release Notes
description: Brief description of the thing
type: docs
weight: 8
---

## 2025-09-26

### 🚀 New Features
- **Confluence Integration**
  - Added Confluence as a new connection type to sync and index content from Confluence pages.
- **GitHub Workflow Run Automations**
  - Introduced support for `workflow_run` events to trigger automations (assessments, PRs, release notes).

### ✨ Enhancements
- **Cross-Platform Comment & Merge Automations**
  - Enabled GitHub issue comments to produce GitLab merge requests and added support for managing GitLab merge-request comments (add, edit, delete, reactions).

### 🐛 Bug Fixes
- **Confluence URL Parsing**
  - Fixed an off-by-one error when extracting page IDs from Confluence URLs to ensure accurate parsing.
- **Document Table UI**
  - Corrected visual layout issues in document tables across the documentation and activity-log pages by improving row-border rendering.
- **Reaction Placement**
  - Fixed reactions to target the last comment event on GitHub issues rather than the entire issue.
- **Automation Reaction Logic**
  - Corrected issue-handling logic to trigger automations accurately based on source vs. documentation repository roles.

---

## 2025-09-19

### 🚀 New Features
- **Experimental Documentation Generation**
  - Trigger documentation updates or create experimental documentation pull requests directly through issue comments labeled `LabelDocHolidayExperimental` or release events.
- **File Bookmarking**
  - Save and revisit important source and target repository files with bookmarking functionality.


### ✨ Enhancements
- **Cross-Platform Automation Enhancements**
  - Create GitLab merge requests from GitHub issues and GitLab issues from GitHub, including GitLab→GitLab pull requests and improved parity across platforms.
  - Retrieve commit ranges and compare tags to commits in GitLab.
- **Dashboard UI Improvements**
  - Added a **Danger Zone** section in the dashboard for deleting publications and connections with clear warnings.
  - Enhanced interactivity and error handling in the publications dashboard when data fetches fail, with integrated error messages and action options.
- **New UI Components**
  - Introduced a **destructive** variant for the Card component to emphasize critical actions.
  - Added support for **Notion** connections with UI fields for naming and integration keys.
  - Added a **Command** component (command palette dialog) for quick access to commands and actions.
  - Renamed **Run Log** to **Activity Log**, with detailed log views, expandable entries, and improved styling.

### 🐛 Bug Fixes
- **Pull Request Logic**
  - Prevented null pointer exceptions in pull request handling by safely accessing file path and content.
- **Navigation Tabs**
  - Fixed tab highlighting in nested routes to ensure the correct active state based on URL.
- **Duplicate Issue Comments**
  - Prevented duplicate automated comments on new issues when source and documentation repositories are the same.
- **Publication Connections**
  - Automatically removed invalid connections during publication updates to prevent errors.

---

## 2025-09-05

### 🚀 New Features
- **Publications Form & Onboarding Enhancements**
  - Implemented a new Publications form with UI components for creating and managing publications and connections.
- **Commit Reversion Support**
  - Added support for reverting individual commits within pull requests, including tools to fetch PR contents and create revert commits.

### 🐛 Bug Fixes
- **Invalid Episode ID Warning Cleanup**
  - Updated system and user templates to return an empty JSON object, resolving persistent warnings from invalid episode ID prompts.


## 2025-08-29

### 🚀 New Features
- **Release-Based Time Ranges**
  - Added support for `release` range types and “since the last release” in time calculations for commits and documentation generation.
- **Google Drive Connections UI**
  - Added buttons and dialog components in the Doc Holiday UI for provisioning Documentation and Google Drive connections.
- **Documentation Site Connections Management**
  - Introduced new dashboard UI components for creating and managing documentation site connections; connection types now include “documentation.”

### ✨ Enhancements
- **Enhanced Encoding Support**
  - Updated the `tiktoken` dependency to v0.1.7 and added an `o200k_base` encoding to support larger token limits.

### 🐛 Bug Fixes
- **Nullable Array Fields**
  - Fixed validation and rendering for nullable array types in the frontend schema.
- **Merge Fail Safeguard**
  - Prevented accidental file loss on document merge failure by returning early to avoid unintended deletions.
- **Skip Non-UTF8 Files**
  - Now skips non-UTF8 encoded text files during synchronization to prevent invalid content storage.
 
### 💼 Known Issues
- **Button Spacing**
  -  Spacing between Skip and Next buttons is inconsistent

---

[Previous entries remain unchanged...]
----------

## Existing Documentation Directory Structure

**Documentation Files (`/content/`):**
- `_index.md` - A summarizing description of Doc Holiday and what it does
- `getting-started.md` - Simple and straightforward steps to deploy and set up Doc Holiday for the first time
- `connections.md` - Describes what sources are supported as connections and how to configure each type  
- `publications.md` - Describes how to create and configure publications and what each option does
- `run-logs.md` - Explains the Activity Logs in the UI and what information is contained in logged activity
- `release-notes.md` - Chronological release notes
- `interactions.md` - Instructions and examples on how the human partner can use Doc Holiday to write documentation
- `style-guide.md` - Explains the basic structures and provide examples of productive and effective style guides that customers can use

**Framework Files:**
- Hugo configuration files at root
- Hextra theme integration
- Static assets and templates

## Existing Source Code Directory Structure

**Sandgarden Source Code Structure (`sandgardenhq/mono/`):**
- **Frontend:** `/js/apps/doc.holiday/` - This is the application user interface written in Next.js UI
- **Backend:** `/sfs/` - This is the entire backend Go service with APIs
- **Shared:** `/kernel/` (Go), `/js/packages/` (JS/TS)
- **Integrations:** `/sfs/connections/` - This is the code for supported sources for connection types
