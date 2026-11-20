# beanstalk-seed

A small spike: put jobs on a [beanstalkd](https://beanstalkd.github.io/) queue and work them from Ruby and from C++, plus a small Rails app for looking at the queue.

> **Status: abandoned experiment, dormant since 2014-11-09.** All three commits were made within half a minute on the morning of 2014-11-09 by Steven Zeiler, during his 2014 Ripple Labs era. Nothing here was verified to run in this pass. Kept as a reference.

## What is in here

| Folder | What it is |
|---|---|
| `ruby/` | `queuer.rb` puts two JSON jobs on the tube `my-tube`; `worker.rb` reserves jobs, prints each one's `key`, and deletes it. Uses the `beaneater` gem. |
| `c++/` | `queuer.cpp` puts one message (from the first argument, or "hello beanstalkd") on the tube `test`; `worker.cpp` reserves, prints and deletes jobs in a loop. Uses a beanstalk C++ client (`beanstalk.hpp`). |
| `web-admin/` | A Rails 4.1.0 app that mounts `BeanstalkdView::Server` at `/beanstalkd` (gem `beanstalkd_view`), a web page for browsing tubes and jobs. |

## Install and run

All of it expects beanstalkd on `localhost` port 11300 (the default).

Ruby:

    cd ruby
    gem install beaneater        # or: bundle install
    ruby queuer.rb
    ruby worker.rb

C++ (needs a beanstalk C++ client library installed so that `beanstalk.hpp` and `-l beanstalk` resolve):

    cd c++
    make                          # builds worker only; the Makefile has no queuer target
    g++ queuer.cpp -o queuer -l beanstalk

Web admin:

    cd web-admin
    bundle install
    bin/rails server              # then open /beanstalkd

The committed `c++/queuer` and `c++/worker` binaries are macOS (Mach-O x86_64) builds and will not run on Linux.

## Configuration

The Ruby and C++ scripts take no environment variables: host, port and tube names are hard-coded. The Rails app reads `SECRET_KEY_BASE` in production (`web-admin/config/secrets.yml`).

## How it works

Producers `put` a job body onto a named tube; workers `reserve` a job, handle it, then `delete` it. The Ruby and C++ halves use different tube names (`my-tube` and `test`), so they do not talk to each other as written.

## Status

See [docs/STATUS.md](docs/STATUS.md). No license is declared.
