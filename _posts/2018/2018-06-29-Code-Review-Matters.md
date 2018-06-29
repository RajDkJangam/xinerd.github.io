---
layout: post
title: Code Review Matters!
categories: [notes]
description: CR 
keywords: Code Review 
---

# Benefits of code reviews

1. Code reviews promote openness
2. Code reviews raise team standards
3. Code reviews propel teamwork
4. Code reviews keep security top-of-mind


# Purpose
1. Does this code accomplish the author’s purpose?

2. Ask questions. 
	 
	Functions and classes should exist for a reason. When the reason is not clear to the reviewer, this may be an indication that the code needs to be rewritten or supported with comments or tests.

# Implementation

1. Think about how you would have solved the problem. 

	If it’s different, why is that? Does your code handle more (edge) cases? Is it shorter/easier/cleaner/faster/safer yet functionally equivalent? Is there some underlying pattern you spotted that isn’t captured by the current code?

2. Do you see potential for useful abstractions?

3. Think like an adversary, but be nice about it.

3. Think about libraries or existing product code.

4. Does the change add compile-time or run-time dependencies (especially between sub-projects)?

# Legibility and style

1. Think about your reading experience. 

	Did you grasp the concepts in a reasonable amount of time? Was the flow sane and were variable and methods names easy to follow? Were you able to keep track through multiple files or functions? Were you put off by inconsistent naming?

2. Does the code adhere to coding guidelines and code style? 

	Is the code consistent with the project in terms of style, API conventions, etc.? As mentioned above, we prefer to settle style debates with automated tooling.

3. Does this code have TODOs? 

	TODOs just pile up in code, and become stale over time. Have the author submit a ticket on GitHub Issues or JIRA and attach the issue number to the TODO. The proposed code change should not contain commented-out code.

# Maintainability
1. Read the tests

	Check the tests themselves: are they covering interesting cases? Are they readable? Does the CR lower overall test coverage? Think of ways this code could break. Style standards for tests are often different than core code, but still important.




# Reference and Links

[Code Review Best Practices](https://medium.com/palantir/code-review-best-practices-19e02780015f)

[Lessons From Google: How Code Reviews Build Company Culture](https://blog.fullstory.com/what-we-learned-from-google-code-reviews-arent-just-for-catching-bugs/)
[Google Java Guide](https://google.github.io/styleguide/javaguide.html)

