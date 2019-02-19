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

**ember-dead-code** hooks into the [central resolver][ember-resolver], logs
all lookups and periodically reports them to your backend in batches. This
technique is called [Real User Monitoring][real-user-monitoring].

[ember-resolver]: https://github.com/ember-cli/ember-resolver
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

## FAQ

#### Why run it in production? Can I not collect this data in my acceptance tests?

There's two problems. If your (acceptance) tests do not cover 100 % of your
codebase, **ember-dead-code** will incorrectly report code as dead that is
actually in use in production. And if your tests _do_ cover 100 % of your
codebase, **ember-dead-code** can incorrectly report code that is dead in
production as still in-use.

You get the most reliable results by monitoring actual production usage.

#### Won't this have a negative performance impact?

The [`ember-resolver`][ember-resolver] is a central piece to every Ember app and
a very hot code path. Hooking into it and doing a lot of synchronous work would
of course degrade performance noticeably.

This is why we try very hard to make the actual data collection as efficient and
lightweight as possible. The only thing we do inside the resolver is pushing the
requested lookup specifier, e.g. `service:foo`, into a shared [`Set`][set], if
natively supported, or plain array otherwise.

[set]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set

We then run a task approx. every `n` seconds (you can configure that value!)
that deduplicates lookup logs and reports them to the backend, when a
configurable threshold batch size is reached.

If available, we use [`requestIdleCallback`][requestidlecallback] for this to
minimize the noticeable performance impact even further. Otherwise we use
[`later`][later].

[requestidlecallback]: https://developer.mozilla.org/en-US/docs/Web/API/Window/requestIdleCallback
[later]: https://emberjs.com/api/api/ember/release/functions/@ember%2Frunloop/later

#### How long do I need to collect data?

YMMV. This depends on the size and behavioral patterns of your user base. We
recommend collecting data for at least two weeks, before drawing any
conclusions. The longer you wait, the more confident you can be in deleting
code.

We also recommend to never stop collecting data. Dead code is an inevitability.
As your codebase evolves over time, you will always forget to remove chunks of
code. It's nice to have some piece of mind, knowing that your dead code will be
reported eventually.
