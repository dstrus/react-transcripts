Hey, everyone. Welcome to Kenzie Academy. I'm Davey Strus.

While we're on the subject of testing, we should probably talk about TDD: Test-Driven Development.

In the previous videos we wrote tests for code that we'd already written. That is, we wrote the code first, and then the tests. In test-driven development, you write the _tests_ first.

Going test-first sort of forces you to rethink your entire development process, so it can be a little scary. It certainly scared me when I first heard about it. How can you possibly write tests for code that doesn't exist yet? You really can, and there are benefits to doing so. Before talking about the benefits, however, we should define what the test-first approach even _is_. So here are the steps:

1. Write a failing test.
2. Make the test pass.
3. Clean it up.

First, write a failing test. Then, write just enough code to make the test pass. Then, clean it up. Refactor.

Tim King wrote a [blog post](http://sd.jtimothyking.com/2006/07/11/twelve-benefits-of-writing-unit-tests-first/) in 2006 titled, "Twelve Benefits of Writing Unit Tests First." I'm not going to go through all 12—for that, you should check out the post itself—but there are a few that really resonate with me.

**You can improve the design without breaking it.** Put another way, tests make refactoring possible. With unit tests, you can refactor with confidence, knowing you can clean up the code without breaking it, because your tests will warn you if things break.

In my pre-testing days, I occasionally did full or nearly-full rewrites, because I saw that a new feature required some redesign of existing code. It wasn't feasible to change only part of that existing code without breaking things. It was all just too tangled up, with too many moving parts and no tests to help keep them straight. With good tests, you'd be amazed by how effectively you can untangle and redesign old code.

**Test-first forces you to plan before you code.** When approaching a new feature, the first question you ask should not be, "What code will I write?" The first thing you ask should be "How will I know when I've solved the problem?" Writing the test first forces you to think through your design and what it must accomplish before you write the code. This not only keeps you focused, it leads to better designs. And...

**It's more fun to code with tests than without.** You know what your code needs to do. Then you make it do it. Even if the integrated app won't run in a browser yet, you can see portions of it run and actually work when you run your tests. Those passing tests reward you when you make each individual thing work. It feels great to turn those things green!

If you'd like to take a stab at test-first, remember the three steps:

1. Write a failing test.
2. Make the test pass.
3. Clean it up.

Kent Beck has a catchy rephrasing of the steps in his book [_Test-Driven Development By Example_](https://www.amazon.com/exec/obidos/ASIN/0321146530/):

1. Red
2. Green
3. Refactor

Red: Write a test that expresses how you'll use the code and what you need it to do. This test will fail.

Green: Write enough code to get the test to pass, but no more. If you need more code, for example, to check for errors, first write another test to demonstrate that feature. For now, just write enough code to get the test to pass.

Refactor: Clean up the code to remove redundancy and improve the design. Then re-run the tests to make sure you didn't break anything.

That's a quick introduction to test-driven development. Writing tests is a skill, and it takes practice. I hope that these last few videos have given you some idea of why it's a good idea, and that you'll make an effort to integrate tests into your development process.

Until next time, I'm Davey Strus. Haaaappy hacking!
