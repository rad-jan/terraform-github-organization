<!--
SPDX-FileCopyrightText: 2020-2022 Mineiros GmbH <hello@mineiros.io>
SPDX-FileCopyrightText: 2025 Radek Janik <cyberwassp@gmail.com>

SPDX-License-Identifier: Apache-2.0
-->

# terraform-github-organization

[![Terraform Version](https://img.shields.io/badge/terraform-1.x-623CE4.svg?logo=terraform)](https://github.com/hashicorp/terraform/releases)
[![Github Provider Version](https://img.shields.io/badge/GH-6.x-F8991D.svg?logo=terraform)](https://github.com/integrations/terraform-provider-github/releases)

## Fork Notice

This is a maintained fork of [mineiros-io/terraform-github-organization](https://github.com/mineiros-io/terraform-github-organization), which is no longer actively maintained. The goal of this fork is to bring the module up to GitHub Provider v6 compatibility and continue ongoing maintenance.

**Attention**: This module requires the `integrations/github` provider and is incompatible with the deprecated `hashicorp/github` provider.

## Warning

**Warning**: As this repository is being brough up to GitHub provider v6, it is
currently not ready for production, development only. Breaking changes can and
will be introduced in the meantime. You've been warned.

## Table of Contents

- [Description](#description)
- [Features](#features)
- [Getting Started](#getting-started)
- [Usage](#usage)
- [Module Reference](#module-reference)
- [Contribute](#contribute)
- [Support](#support)
- [Contact](#contact)
- [Credits](#credits)
- [Legal](#legal)
- [Versioning](#versioning)

## Description

A [Terraform](https://www.terraform.io) module for managing [GitHub Organizations](https://github.com/) including members, teams, projects, and organization settings.

This module supports Terraform v1.x and is compatible with the Official Terraform GitHub Provider v6.x from `integrations/github`.

Originally developed by [Mineiros](https://github.com/mineiros-io), this fork aims to:

- Update compatibility to GitHub Provider v6
- Fix outstanding issues and bugs
- Continue active maintenance and development

In contrast to using individual GitHub provider resources, this module provides a convenient wrapper that manages organization members, admins, blocked users, projects, and settings in a cohesive and tested way.

## Features

- **Default Security Settings**: Restrictive defaults for member permissions and repository creation
- **Standard Organization Features**: Organization members, organization owners (admins), organization projects, blocked users, organization settings
- **Extended Organization Features**: Change member roles without removing and re-inviting users, rename projects without recreating (when providing unique ids), no need to import members/admins on first run, create an all-members team containing every member of your organization

## Getting Started

Most basic usage managing a GitHub organization:

```hcl
module "organization" {
  source  = "rad-jan/organization/github"
  version = "~> 0.9.0"

  members = ["member-user"]
  admins  = ["admin-user"]

  settings = {
    billing_email = "billing@example.com"
  }
}
```

## Usage

See [variables.tf](variables.tf) and [examples/](examples/) for detailed use-cases.

The intended behavior is to enforce the state of your organization and its configurations. If you modify resources outside of Terraform and reapply the state, conflicting configurations will be overwritten.

For comprehensive documentation on module arguments, outputs, and advanced configurations, see [MODULE_REFERENCE.md](docs/MODULE_REFERENCE.md).

### Quick Reference

| Argument | Description | Default |
|----------|-------------|---------|
| `settings` | Organization settings object | `{}` |
| `members` | List of member usernames | `[]` |
| `admins` | List of admin usernames | `[]` |
| `blocked_users` | List of blocked usernames | `[]` |
| `projects` | Organization projects | `[]` |
| `all_members_team_name` | Name for auto-managed all-members team | `""` |
| `all_members_team_visibility` | Visibility of all-members team (`secret` or `closed`) | `"secret"` |
| `catch_non_existing_members` | Validate users exist (adds API calls) | `false` |

### Organization Settings

```hcl
module "organization" {
  source = "rad-jan/organization/github"

  settings = {
    billing_email                                                = "billing@example.com"
    company                                                      = "My Company"
    blog                                                         = "https://blog.example.com"
    email                                                        = "contact@example.com"
    location                                                     = "City"
    name                                                         = "My Organization"
    description                                                  = "Organization managed by Terraform"
    has_organization_projects                                    = true
    has_repository_projects                                      = true
    default_repository_permission                                = "read"
    members_can_create_repositories                              = false
    members_can_create_public_repositories                       = false
    members_can_create_private_repositories                      = false
    members_can_create_internal_repositories                     = false
    members_can_create_pages                                     = false
    members_can_create_public_pages                              = false
    members_can_create_private_pages                             = false
    members_can_fork_private_repositories                        = false
    web_commit_signoff_required                                  = true
    advanced_security_enabled_for_new_repositories               = true
    dependabot_alerts_enabled_for_new_repositories               = true
    dependabot_security_updates_enabled_for_new_repositories     = true
    dependency_graph_enabled_for_new_repositories                = true
    secret_scanning_enabled_for_new_repositories                 = true
    secret_scanning_push_protection_enabled_for_new_repositories = true
  }
}
```

### Members and Projects

```hcl
module "organization" {
  source = "rad-jan/organization/github"

  settings = {
    billing_email = "billing@example.com"
  }

  # Organization members
  members = [
    "developer-1",
    "developer-2",
  ]

  # Organization admins
  admins = [
    "org-admin",
  ]

  # Blocked users
  blocked_users = [
    "spammer",
  ]

  # Organization projects
  projects = [
    {
      id   = "roadmap"
      name = "Product Roadmap"
      body = "Track product development milestones"
    }
  ]

  # Create a team containing all members
  all_members_team_name       = "Everyone"
  all_members_team_visibility = "closed"
}
```

## Module Reference

For the complete module argument reference, outputs, and advanced configurations, see [docs/MODULE_REFERENCE.md](docs/MODULE_REFERENCE.md).

### External Documentation

- [Terraform GitHub Provider - Organization Settings](https://registry.terraform.io/providers/integrations/github/latest/docs/resources/organization_settings)
- [Terraform GitHub Provider - Membership](https://registry.terraform.io/providers/integrations/github/latest/docs/resources/membership)
- [Terraform GitHub Provider - Organization Block](https://registry.terraform.io/providers/integrations/github/latest/docs/resources/organization_block)
- [Terraform GitHub Provider - Team](https://registry.terraform.io/providers/integrations/github/latest/docs/resources/team)

## Contribute

Contributions are welcome! See [CONTRIBUTING.md](CONTRIBUTING.md) for contribution guidelines.

Areas where help is especially appreciated:

- Testing with GitHub Provider v6
- Documentation improvements
- Bug fixes and feature additions

## Support

Support is provided on a best effort basis. If you encounter issues or have feature requests, please open an issue on GitHub. The project is best developed in the open so anyone can search for past issues and solutions.

## Contact

- [GitHub Issues](../../issues) - Bug reports and feature requests
- [GitHub Discussions](../../discussions) - Questions and community discussion

## Credits

This module was originally developed by [Mineiros GmbH](https://mineiros.io). Their work on Terraform modules for GitHub management laid the foundation for this project.

Thanks to all contributors to both the original repository and this fork.

## Legal

This project is [REUSE-compliant](https://reuse.software).

The easiest way to get the copyright and license of the project is with the reuse tool:

```sh
reuse spdx
```

You can also check this information manually by looking in the file header, a companion `.license` file, or in `.reuse/dep5`.

All licenses are present in the `LICENSES` directory.

## Versioning

This project follows [Semantic Versioning (SemVer)](https://semver.org/):

- `MAJOR` version for incompatible changes
- `MINOR` version for backwards compatible functionality
- `PATCH` version for backwards compatible bug fixes
