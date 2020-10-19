# Isildur's Bane

## Goals

This is a template for 18F and TTS guides, built with the following goals and assumptions:

- Guides need minimal stylistic customization beyond the stock usdws-jekyll template.
- Guide maintainers want to focus on content, not servicing dependabot and Snyk PRs.
- Guides at 18F and TTS should have a similar layout/branding/functionality and be compliant with GSA policies.
- Guides can be well-served by a common set of Jekyll plug-ins, components, CircleCI test scripts, etc.

Some guides at 18F/TTS might be exceptions; e.g., the TTS handbook (large volume of content) and 18F Methods Guide (content demands different visual layout).

Once this template is incorporated into a guide GitHub repo, guide maintainers can ignore all the supporting files and solely focus on:
- README.md
- The _guide folder, which has:
  - simplified _config.yml
  - _data folder, with simplified data files
  - _pages folder for Markdown content

Security and dependency updates will automatically be applied to their guide repository.

## Relevant discussions

- [state of TTS documentation sites - 2020-03-28](https://docs.google.com/document/d/1LCKZKqoEEYOCwntZpTbHp8bW7tdnPnmmKVJ-YNHAUy4)
- [GSA guidelines thread in #practice-leads](https://gsa-tts.slack.com/archives/C6W0B5W1G/p1598380975005700)
- [18F/uswds-jekyll proposal: make more opinionated](https://github.com/18F/uswds-jekyll/issues/190)

## Technical details

To achieve these goals, this repository uses:

- [usdws-jekyll](https://github.com/18F/uswds-jekyll)
- Styling from [`18F/ux-guide`](https://github.com/18F/ux-guide) and [`18F/federalist-jekyll-uswds-18f-port`](https://github.com/18F/federalist-jekyll-uswds-18f-port)
- Code from [`18F/before-you-ship`](https://github.com/18F/before-you-ship) and [`18F/handbook`](https://github.com/18F/handbook)
- [a GitHub Action](./.github/actions/merge-template/action.yml) that downstream guides will (automatically) call to merge updates from this upstream repository
- a guide-specific directory to put configuration and content and simplify the "API" a guide maintainer needs to navigate:
  - [`_guide/_config.yml`](./_guide/_config.yml), augments the Jekyll/uswds-jekyll `_config.yml`
  - [`_guide/_data`](./_guide/_data) for only the YAML files and keys that can be altered downstream
  - [`_guide/_pages`](./_guide/_pages) 
- a separate [`_data/orgs`](./_data/orgs) subdirectory and [site configuration key](https://github.com/18F/isildurs-bane/blob/77b8aece41f4f61988a40cc079a70d07670c11e5/override.yml#L25) to have optional 18F, Solutions, CoE, or PIF branding. TTS-only branding is enabled by default.
- [a Jekyll generator](./_plugins/override.rb) to override values set from YAML files in `_data` and in `_config.yml`
  - Note: Federalist does not support multiple Jekyll config files (e.g., `--config _config.yml,_guide/_config.yml`), requiring custom code
- [Git attributes](./.github/actions/merge-template/action.yml#L8) to help ensure this upstream repository does not affect downstream content in `_pages`, etc.
- `Template repository` setting in GitHub

## Concept of operations

1. Use this repository as a template to create a new repository. Alternatively, merge its content into an existing repository.
1. Edit `_guide/_config.yml`, `_guide/_data/navigation.yml`, `_guide/_data/header.yml`, `_guide/_data/anchor.yml`, `README.md`, and content under `_guide/_pages`. Don't edit anything else if you want to guarantee consistency across guides and automatic updates for security and compliance.
1. Set up Federalist, CircleCI, Snyk, etc.
1. Register sitemap in search.gov.
1. Any security updates, configuration changes, style changes done by the template maintainer get automatically merged into the downstream guide (i.e., once a day).
1. Downstream guides are automatically redeployed with up-to-date dependencies.
1. It is anticipated that eventually there will be non-backwards compatible changes to Jekyll. At that point, some manual intervention might be required.

## Development

Once you have instantiated a downstream guide repository and cloned it locally, use Docker to view your site:

1. ```sh
   docker-compose up --build
   ```

1. Open http://localhost:4000
