# ember-dead-code

[![Build Status](https://travis-ci.org/buschtoens/ember-dead-code.svg)](https://travis-ci.org/buschtoens/ember-dead-code)
[![npm version](https://badge.fury.io/js/ember-dead-code.svg)](http://badge.fury.io/js/ember-dead-code)
[![Download Total](https://img.shields.io/npm/dt/ember-dead-code.svg)](http://badge.fury.io/js/ember-dead-code)
[![code style: prettier](https://img.shields.io/badge/code_style-prettier-ff69b4.svg)](https://github.com/prettier/prettier)  
[![Ember Observer Score](https://emberobserver.com/badges/ember-dead-code.svg)](https://emberobserver.com/addons/ember-dead-code)
[![Ember Versions](https://img.shields.io/badge/Ember.js%20Versions-%5E2.18%20%7C%7C%20%5E3.0-brightgreen.svg)](https://travis-ci.org/buschtoens/ember-dead-code)
[![ember-cli Versions](https://img.shields.io/badge/ember--cli%20Versions-%5E2.13%20%7C%7C%20%5E3.0-brightgreen.svg)](https://travis-ci.org/buschtoens/ember-dead-code)  
[![Dependabot enabled](https://img.shields.io/badge/dependabot-enabled-blue.svg?logo=dependabot)](https://dependabot.com/)
[![dependencies](https://img.shields.io/david/buschtoens/ember-dead-code.svg)](https://david-dm.org/buschtoens/ember-dead-code)
[![devDependencies](https://img.shields.io/david/dev/buschtoens/ember-dead-code.svg)](https://david-dm.org/buschtoens/ember-dead-code)

Detects dead code with confidence via real user monitoring. Finds dead
components, routes, helpers, ember-intl translations and whatever your heart
desires!

## The Idea

JavaScript is a dynamic language and notoriously hard to statically analyze.
While it is easy to detect that a certain file or piece of code _is used_,
detecting that it _is not used_ is really hard. The fact that components and
helpers don't get invoked through JavaScript, but Handlebars, and might even
have interpolated names, makes this virtually impossible.

False positives are dangerous, as they could lead to deletions that turn up
weeks later in production, because of some weird edge-case scenario that has not
been accounted for in the analysis.

So instead of doing the hard work ourselves, why don't we let our users do the
work in production for us?

**ember-dead-code** hooks into the [central resolver][ember-cli-resolver], logs
all lookups and periodically reports them to your backend in batches. This
technique is called [Real User Monitoring][real-user-monitoring].

[ember-cli-resolver]: https://github.com/ember-cli/ember-resolver
[real-user-monitoring]: https://en.wikipedia.org/wiki/Real_user_monitoring

The longer you collect data, the more confident you can become that your list of
code that is in-use is complete. You can then feed this report back into
**ember-dead-code** and let it remove dead code for you.

## Installation & Setup

```
ember install ember-dead-code
```

## Development Status

So far this is just an idea and not functional yet. This repo exists so that we
can already talk about the concept and review some code.
