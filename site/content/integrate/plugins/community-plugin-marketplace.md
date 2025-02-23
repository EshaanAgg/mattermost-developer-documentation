---
title: Community plugins in the Marketplace
heading: "Community plugins in the Marketplace"
description: "Once your plugin has reached a certain level of quality, you might consider submitting it to the Marketplace."
weight: 110
aliases:
  - /extend/plugins/community-plugin-marketplace/
---

Once your plugin has reached a certain level of quality, you might consider submitting it to the Marketplace. The Marketplace is a platform that supports discovery, installation, and updates of plugins directly within Mattermost. It's a great way to get feedback on your plugin and help make it more popular. Once your plugin is accepted to the Marketplace, Mattermost will also send you swag!

### Requirements for adding a community plugin to the Marketplace

Every community plugin must fulfill the following checklist to be added to the Marketplace:

**Product requirements (checked by a Product Manager)**

1. The plugin is published under an [Open Source license](https://opensource.org/licenses/alphabetical).
2. The source code is available in a public Git repository.
3. There is a public issue or bug tracker for the plugin, which is linked in the plugin documentation and linked via `support_url` in the manifest.
4. For the current release and future releases, a changelog must be published. The link to the current release notes has to be recorded in the `release_notes_url` property of the `plugin.json` manifest. For example GitHub releases can be used to publish the changelog.
5. The plugin has to be out of Beta and be released with at least v1.0.0.
6. All configuration is accessible via the Mattermost interface.
7. The plugin ID defined in the manifest must not collide with the ID of an existing plugin in the Marketplace. It should follow [the documentation's suggested naming convention]({{< ref "/integrate/plugins/manifest-reference#id" >}}).

**Technical requirements (checked by the relevant development team)**

1. The plugin works for 60k concurrent connections and in a High Availability deployment. **Note:** There are currently no publicly-available tools to verify these properties. As such, they are checked during code review by a developer.
2. The plugin logs important events on appropriate log levels to allow System Admins to troubleshoot issues.

**Security requirements (checked by a member of the Security team)**

1. Security reviews do not reveal any exploitable vulnerabilities in the plugin.
2. The plugin provides an email address or a username on the [Community Server](https://community.mattermost.com) used to report vulnerabilities in the future.

**Functional requirements (checked by a QA tester)**

1. The plugin must set a `min_server_version` in the manifest.
2. The plugin must work on all Mattermost versions greater than or equal to the `min_server_version`.

**Documentation requirements (checked by a Technical Writer)**

1. The plugin must include detailed usage documentation with at least one screenshot of the plugin in action, list of features, and a development guide. This is typically a `README` file or a landing page on the web. The link to the documentation is set as `homepage_url` in the manifest. A great example is the [`README` of the GitHub plugin](https://github.com/mattermost/mattermost-plugin-github/blob/master/README.md). Typical components of documentation include:

    * Requirements/Prerequisites
    * Installation steps
    * Configuration steps
    * Usage
    * Troubleshooting
    * Screenshots (if available)
    * Link or email address for help/support

2. The `plugin.json` file should include formatting consistent with the System Console interface. You can join the [Documentation channel](https://community.mattermost.com/core/channels/documentation) for assistance.

Please note that Mattermost reserves the right to reject any plugin submission from the Marketplace.


### Requirements for updating a community plugin on the Marketplace

When a community plugin is updated, the new version must fulfill the following checklist to remain on the Marketplace. The new version checked by the four reviewers in the same way as when the plugin was added. The code review and security review should be performed against the `diff` of the last version in the Marketplace and the new version to be updated in the Marketplace.

The release also has to follow [Semantic Versioning](https://semver.org/). For plugins this means:

* If the plugin exposes a public API, breaking changes to the API require a major version bump.
* If an update requires manual migration actions from the System Admin, a major version bump is required.

This is checked in dev review.

The new release must not change the plugin ID defined in the manifest as this would require a reconfiguration of the plugin by a System Admin.

### Process for adding a community plugin to the Marketplace

All community plugins are assigned an _owner_ to guide you through the review process. Connect with [hanzei](https://github.com/hanzei) for more details. Ask non-confidential questions in the [Marketplace channel](https://community.mattermost.com/core/channels/plugins-marketplace).

1. Open an issue on the Marketplace repository using [a pre-defined template for new plugins](https://github.com/mattermost/mattermost-marketplace/issues/new?template=add_plugin.md). The template contains the checklist above, so you can check the items. Please also point out which commit should be used for the review. You may cut a release candidate (RC) for the reviews.
2. The _owner_ forks the community repository under the Mattermost GitHub organization as a private fork so the existing build tools for releasing new plugin versions can be used. The fork is maintained by the _owner_. Naming conflicts are resolved by appending your username to the repository name e.g. `jira-someusername`. The community member is given read access to the private fork.
3. The _owner_ submits a pull request to merge the latest version of the community plugin into `master`. Reviews are requested by the _owner_. The reviewers point out general discovered issues in the pull request or on the bug tracker of the community plugin. Once all blocking issues are resolved, they approve the pull request.
4. The pull request gets merged and`/mb cutplugin --repo $REP --tag $TAG` is run to build, sign, and upload the approved commit of the plugin.
5. The _owner_ opens a pull request, which adds the plugin to `plugins.json` using `generator add $REP $TAG --community`. Only a functional review by one dev and one QA member is needed for this pull request.
6. After the pull request is merged, the plugin gets promoted across Mattermost social media and swag is sent to the maintainer. If there are multiple maintainers, everyone gets swag.

### Process for updating community plugin in the Marketplace

1. Open an issue on the Marketplace repository using [a pre-defined template for existing plugins](https://github.com/mattermost/mattermost-marketplace/issues/new?template=update_plugin.md). The template contains the checklist above, so you can check the items. Please also point out which commit should be used for the review. You may cut a release candidate (RC) for the reviews.
2. The  _owner_ submits a pull request to merge the upstream changes. Reviews are requested by the _owner_. The reviewers point out general discovered issues in the pull request or on the bug tracker of the community plugin. After all blocking issues are resolved, they approve the pull request.
3. The pull request gets merged and `/mb cutplugin --repo $REP --tag $TAG` is run to build, sign, and upload the approved commit of the plugin.
4. The _owner_ opens a pull request, which adds the plugin to `plugins.json` using `generator add $REP $TAG --community`. Only a functional review by one dev and one QA member is needed for this pull request.
5. Promotion via social media might happen on outstanding updates.

### Beta plugins

If a community plugin doesn’t make it through the review process, it may still be added to the Marketplace and marked as “Beta”. The reviewers decide whether the quality of a plugin is sophisticated enough to be added to the Marketplace on a case-by-case basis. Security and functional reviews and items 1, 2, 3, and 5 from the [Product Requirements Checklist](#requirements-for-adding-a-community-plugin-to-the-marketplace) must be fulfilled for Beta plugins.

### Security issues

Any security issues found in the plugin should be reported by email to `responsibledisclosure@mattermost.com` or sent directly to a member of the [Security team]({{< ref "/internal/rd-teams#security-team" >}}) on the [Community Server](https://community.mattermost.com/).

### Take down policy

If an medium or greater security issue or bug that prevents the usage of the plugin for many users is not fixed within 14 days, the plugin will be removed from the Marketplace. It may be resubmitted once the issue is resolved. Mattermost reserves the right to take down plugins at any time if a fix for a security issue is not forthcoming or the issue is critical enough to justify an immediate take down.
