# Home Page of Junfeng Hu

Personal technical blog built with [Jekyll](https://jekyllrb.com/) and the [Minimal Mistakes](https://mmistakes.github.io/minimal-mistakes/) theme.

## Prerequisites

- [mise](https://mise.jdx.dev/) — version manager for Ruby
- Ruby 3.3.11 (managed by mise via `mise.toml`)
- Bundler

## Local Development

```bash
# 1. Install Ruby (if not already installed via mise)
mise install

# 2. Install gem dependencies
bundle install

# 3. Start the dev server
bundle exec jekyll serve

# Or with live reload and accessible on the network
bundle exec jekyll serve --livereload --host 0.0.0.0
```

The site will be available at **http://localhost:4000**.

## Project Structure

```
├── _config.yml          # Site configuration
├── _pages/              # Static pages
├── _posts/              # Blog posts
├── _data/               # Data files (navigation, etc.)
├── _sass/               # Sass/SCSS partials
├── assets/              # Images, CSS, JS
├── Gemfile              # Ruby gem dependencies
└── mise.toml            # Ruby version (via mise)
```

## Deployment

The site is deployed via GitHub Pages. Pushing to the `master` branch triggers an automatic build and deploy.
