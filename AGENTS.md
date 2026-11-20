# AGENTS.md

## What this is

`beanstalk-seed` is a small 2014 experiment with beanstalkd queues in Ruby and C++, plus a Rails 4.1 admin app. Dormant since 2014-11-09. There is no library and no product here.

## Stack

- Ruby with the `beaneater` gem (`ruby/Gemfile`). Needs a running beanstalkd on `localhost:11300`.
- C++ with a beanstalk client library providing `beanstalk.hpp` (not vendored), built with `g++`.
- `web-admin/`: Rails 4.1.0, `beanstalkd_view`, sqlite3, `sass-rails ~> 4.0.3`. Rails 4.1 does not run on modern Ruby; expect `bundle install` to fail on a current toolchain. Not tried in this pass.

## Layout

- `ruby/`: `queuer.rb`, `worker.rb`, `Gemfile`.
- `c++/`: `queuer.cpp`, `worker.cpp`, `Makefile` (builds `worker` only), two committed macOS binaries, one stray vim swap file.
- `web-admin/`: generated Rails skeleton with one route to `BeanstalkdView::Server`.
- `docs/STATUS.md`: current state.

## Commands

- Ruby: `cd ruby && ruby queuer.rb` then `ruby worker.rb` (with beanstalkd running).
- C++: `cd c++ && make` (worker only).
- Rails: `cd web-admin && bundle install && bin/rails server`. Known not to work on today's toolchain.
- Tests: none. `web-admin/test/` holds only the Rails generator's empty folders.

## Rules

- Add or update tests with every change. If a change cannot be tested, say why in the pull request.
- Never commit secrets: keys, tokens, passwords, `.env` files. Use placeholders such as `sXXXXXXXX`.
- Use Conventional Commits (`feat:`, `fix:`, `docs:`, `chore:`).
- Keep `docs/STATUS.md` current when behaviour or tooling changes.

## What agents may and may not do

- May: fix bugs, improve docs, add tests, open pull requests against `master`.
- May not, without Steven: archive or delete the repo, publish to npm or any registry, rewrite git history, change the license, or merge their own pull requests.
- Never put real values in `web-admin/config/secrets.yml`: it is committed (see STATUS). Production reads `SECRET_KEY_BASE` from the environment.
