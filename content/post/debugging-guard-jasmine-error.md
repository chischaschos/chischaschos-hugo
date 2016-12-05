+++
date = "2016-09-27T11:11:11-07:00"
title = "Debugging guard-jasmine error"

+++

## Motivation

I'm working on a project that uses `guard-jasmine (1.19.2)`. For a number reasons I can't upgrade it yet.

This morning the test headless test suite started failing with:

```
➜  bundle exec guard-jasmine
Guard::Jasmine starts Unicorn spec server on port 58583 in test environment (coverage off).
Waiting for Jasmine test runner at http://localhost:58583/jasmine
Run all Jasmine suites
Run Jasmine suite at http://localhost:58583/jasmine
2016-09-27 22:01:55.025 phantomjs[3242:3209481] *** WARNING: Method userSpaceScaleFactor in class NSView is deprecated on 10.7 and later. It should not be used in new applications. Use convertRectToBacking: instead.
An error occurred in the Jasmine runner
```

It is hard to notice but the actual error is the last line: **An error ocurred in the Jasmine runner**. If I run the tests in the browser they are fine,
only the the headless suite is failing.

I got a warning and a message that does not give me too much details about what is going on. The following is the set of steps that I followed to find the error.

## The bug hunt
I started asking myself a bunch of questions such as:

### Q1. Can I execute the same command and get a more verbose output?

  ```
  ➜  bundle exec guard-jasmine --verbose
Guard::Jasmine starts Unicorn spec server on port 58749 in test environment (coverage off).
I, [2016-09-27T22:06:51.953642 #3742]  INFO -- : listening on addr=0.0.0.0:58749 fd=11
I, [2016-09-27T22:06:51.953792 #3742]  INFO -- : worker=0 spawning...
I, [2016-09-27T22:06:51.954728 #3742]  INFO -- : master process ready
I, [2016-09-27T22:06:51.955798 #3743]  INFO -- : worker=0 spawned pid=3743
I, [2016-09-27T22:06:51.956231 #3743]  INFO -- : Refreshing Gem list
Waiting for Jasmine test runner at http://localhost:58749/jasmine
** [Honeybadger] Unable to start Honeybadger -- api_key is missing or invalid. level=2 pid=3743
OpenIdAuthentication.store is nil. Using in-memory store.
I, [2016-09-27T22:06:57.459446 #3743]  INFO -- : worker=0 ready
Run all Jasmine suites
Run Jasmine suite at http://localhost:58749/jasmine
2016-09-27 22:06:59.227 phantomjs[3793:3213439] *** WARNING: Method userSpaceScaleFactor in class NSView is deprecated on 10.7 and later. It should not be used in new applications. Use convertRectToBacking: instead.
An error occurred in the Jasmine runner
  ```

**A1:** Well yes there is a verbose option, but it didn't things clearer for me.

### Q2. Can I set a debugger and see what is going on?

**A2:** Well yes but where?. OK so the entry point is the command `guard-jasmine` so:

- https://github.com/guard/guard-jasmine/blob/v1.19.2/bin/guard-jasmine#L3
- Means it calls `Guard::Jasmine::CLI.start`
- The CLI is a [Thor](http://whatisthor.com/) implementation, which ends up calling:
- https://github.com/guard/guard-jasmine/blob/v1.19.2/lib/guard/jasmine/cli.rb#L216
- Meaning it calls [::Guard::Jasmine::Server.start(runner_options)](https://github.com/guard/guard-jasmine/blob/v1.19.2/lib/guard/jasmine/cli.rb#L220)
- So it first starts a *server* and then runs a *command*.
- So here [::Guard::Jasmine::Runner.run(paths, runner_options)](https://github.com/guard/guard-jasmine/blob/v1.19.2/lib/guard/jasmine/runner.rb#L44) and then [run_jasmine_spec](https://github.com/guard/guard-jasmine/blob/v1.19.2/lib/guard/jasmine/runner.rb#L109)
- I put a debugger in [line 108](https://github.com/guard/guard-jasmine/blob/v1.19.2/lib/guard/jasmine/runner.rb#L108) and then checked the command that is was executing:

```
/usr/local/bin/phantomjs /Users/blabla/.rbenv/versions/2.3.1/lib/ruby/gems/2.3.0/gems/guard-jasmine-1.19.2/lib/guard/jasmine/phantomjs/guard-jasmine.js \"http://localhost:59100/jasmine\" 30000000 always true failure failure false false ''
```

So the command it executes has the form:

- *phantomjs-binary* *guard-jasmine-js-script* *guard-jasmine-js-arguments*

And the guard-jasmine arguments are:

- *instance-running-jasminerice* *more-args-to-guard-jasmine-script*

**A2 Extended answer:** Now I am able to run the same command without invoking the gem executable (copy paste it from the debugger), the problem is that I get the same error.
The nice thing is that I learned how test runner works.

### Q3: Before keep digging, are you sure your JS does not contain any errors?

Let me pass my JS to node and let it do a syntax check:

```
wget -qO-  http://localhost:3000/assets/app.js | node
[stdin]:2
  Namespace('utils');
  ^

ReferenceError: Namespace is not defined
    at [stdin]:2:3
    at [stdin]:42:3
    at ContextifyScript.Script.runInThisContext (vm.js:26:33)
    at Object.exports.runInThisContext (vm.js:79:17)
    at Object.<anonymous> ([stdin]-wrapper:6:22)
    at Module._compile (module.js:571:32)
    at Immediate.<anonymous> (bootstrap_node.js:383:29)
    at runCallback (timers.js:649:20)
    at tryOnImmediate (timers.js:622:5)
    at processImmediate [as _immediateCallback] (timers.js:594:5)
```

I don't thinkg this is helpful at all. Instead I'm gonna pass it through a linter using the default configurations:

```
wget -qO-  http://localhost:3000/app/assets/app.js | eslint --stdin --no-eslintrc

<text>
  1222:35  error  Parsing error: Unexpected token =

✖ 1 problem (1 error, 0 warnings)
```

This looks nicer so now let's see what that line has:

```
wget -qO-  http://localhost:3000/app/assets/app.js | sed -n '1222p'
  initialize: function(attributes = {}) {
```

**A3:** I'm using a default argument. Which worked fine in the browser, but not in my old headless test suite.

## Solution
I didn't need a default argument right there. So I simply removed it. Anyways I really need to upgrade my test
suite.
