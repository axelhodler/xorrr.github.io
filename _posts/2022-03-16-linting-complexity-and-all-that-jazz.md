---
layout: post
title: "Linting, complexity and all that jazz"
date:   2022-03-16 06:30:22
description: ""
tags: []
---

Checking out a new, at least to me, python codebase where the `README.md` stated

{% highlight bash %}
flake8 src/ tests/
black src/ tests/
{% endhighlight %}

[Flake8](https://flake8.pycqa.org/en/latest/index.html) is a tool for style guide enforcement. Often called a linter. A static code analysis tool.
While [black](https://github.com/psf/black) is a python code formatter.

Cool. Let's run these commands.

{% highlight bash %}
flake8 src/ tests/
{% endhighlight %}

Executed successfully. No warnings. No errors. Thumbs up. On with

{% highlight bash %}
black src/tests
{% endhighlight %}

Executed successfully. Has reformatted a single file. Cool.

Before the reformat there was the following line. (Think of it as one line even if this blog setup splits it into two)

{% highlight python %}
def validate_action(self, currentState: TestRunAction, action: TestRunAction):  # noqa: C901
{% endhighlight %}

The reformat did apply the maximum line length rule and changed the above to

{% highlight python %}
def validate_action(
  self, currentState: TestRunAction, action: TestRunAction
):  # noqa: C901
{% endhighlight %}

Ok cool, LGTM, commit and push.

Seconds later I get back a failed pipeline result.

Why?

> src/backend/processor.py:335:5: C901 'Processor.validate_action' is too complex (14)

Let's check [C901](https://www.flake8rules.com/rules/C901.html).

Ok, that's what the `# noqa C901` comment did.
It told `flake` to ignore the configured complexity rules for the `validate_action` method.

Awesome now I can either:

* Decrease the complexity of `validate_action`
* Google where else to put the `# noqa: C901` comment - All while taking into account the method now being split into three lines

Plus I'm left wondering why there was a file checked into mainline which did not follow the code formatting rules.

Turns out the `black` formatting is not part of the pipelines. But `flake8` is. Explains the failure above.

Fine, the pipeline should probably not reformat the code automatically. As a result it would have to create its own reformat commits.

Two additional todos get added to my backlog:

* Find out how to fail the build if `black` has to format - need to google that - because if there is a reformat `black` will just reformat and exit with code 0 (success).
* Create a git hook to run the command on localhost before a commit. Or to tell the editor to automatically apply `black` during save.

Additionally we notice the `README.md` states the two commands in the wrong order. `black` should have been run first to catch the error before creating a commit.

A third TODO appears.

* improve documentation

Now without having fixed the actual issue, the complexity of `validate_action`, we have already gathered three additional todos.

We enter the common dilemma on whether to be a good sport and fix these issues or to just not give a f\*ck, undo the formatting, push changes and have the green build again.

(And even if we give a f\*ck we might want to undo the formatting - to be back to green and then to fix all the above)

We could quickly improve the readability of `validate_action`, achieved by extracting the logical parts into methods. Sadly extracting methods does not improve complexity, only readability. Great example for the difference between complexity and being complicated.

Or we could change the rules to increase the max characters per line limit. Or to allow more complexity in methods. Both would have to be discussed with the team.

Quickly removing the types `TestRunAction` to shorten the line and have the necessary space to fit `# noqa: C901` there?

Great, making the code worse to game the linter.

We can take the above as an example why forcing formatting rules on a team doesn't just magically achieve the same formatting everywhere.
And why `flake8` does not automatically improve code quality. It can simply be disabled as it was in the case above. As a result we get cryptic ignore statements throughout our code base that probably no one will ever fix.

> So what you are saying is these tools don't help us?

Yes. I believe so.

Although, f\*ck, it depends.

If the team really cares we don't need a linter. Errors would be caught by tests and formatting would be applied by the editor. Everyone would not want to have their code differ from what is already in the code base.

If the team does not care too deeply these tools only add additional friction. Leading to ignore statements throughout the code. Increased build time plus commits such as "satisfy linter". And of course a team that hates the rules but does not want to bother with having a discussion about changing them.

Especially if there are folks on the team who peruse and try these rules with the same energy a kid would apply to the candy in a candy store. Have them all.

To add insult to injury - Both tools did not help us to avoid having both snake_case (the `validation_action`) and camelCase (the `currentState`).

Probably another rule available in the docs of `flake8`. Haha.
