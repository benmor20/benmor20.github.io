# Noodle Maps

In my junior year, I took Larger-Scale Software Development, a class that looks into how software development is done when a large team of people must work together to write a single piece of software. We looked into proper commenting, testing, and continuous integration (CI) frameworks. As a final project, two classmates and I decided to make our own CI through GitHub. We explored this through the lens of a web app we called noodle maps, which would allow a user to find the optimal route from a starting to an ending location while hitting multiple stops along the way in any order. We the app itself was largely a backend through the Google Maps API with a very basic HTML form for the frontend, as the focus of the project was the CI. You can find our work on [the noodle maps repo](https://github.com/olincollege/noodlemaps).

## Our Framework

Our first step was to decide on a CI method that allowed us to operate efficiently while still writing high-quality code. We decided that our workflow for a new feature would look like this:

1. Create a branch for the new feature
2. Create unit tests (tests on the feature itself) and integration tests (tests on how the feature will interact with other features). This is done before actually implementing the feature so that the tests must be on that actual inputs and outputs of the feature. It would not be possible to write implmentation specific tests (which are more brittle and prone to errors should refactoring occur), as the implementation would not yet be made.
3. Build the feature. As commits are made to the branch, a commit hook will pop up a message to us if the unit tests are failing.
4. Once all unit tests pass, submit a PR to `true-head`, and assign others to review it. GitHub will then run an action we made to verify that the integration tests also pass. If they do not, the PR may not be merged before the errors are fixed. Note that this PR process is the only way to merge to `true-head`.
5. Once the PR is approved (all tests pass and reviewers okay it), it is merged into `true-head`. GitHub will then run a second action we made to ensure that the entire script is working properly, end-to-end. If it is, a merge will automatically be made into `green-head`, the main branch. This is the only way to update `green-head`. Since unit tests, integration tests, and end-to-end tests must pass to reach this point, we can be fairly certain that our main branch is free of major bugs.

This sequence of checks is made with the goal of preventing bugs as soon as they are detected, and minimizing the number of bugs that can make it into the main branch. There are several checks that must pass before any change to the main branch is made - several sets of automatic tests must pass and it must be okayed by other developers. Additionally, it is not possible to accidentally merge into `green-head`, or even `true-head`, as our repo is set up to only allow merges from these automatic actions.

## My Work

While I did do some feature implementation (specifically, [the `UrlBuilder` class](https://github.com/olincollege/noodlemaps/blob/42df3cc9a49b3fb977f1ede1a08290c1d39a91aa/src/utils/url_builder.py)), my main role in this project was creating GitHub actions for our CI framework.

### Integration Tests

I first wrote a [GitHub Action to perform integration tests](https://github.com/olincollege/noodlemaps/blob/4c2eaeef568934575e3a34088523362874a23937/.github/workflows/integration-tests.yml). I set up our repo so that, should this Action fail, the PR may not proceed, effectively blocking any exposed bugs. This Action actually caught a few bugs for us (for example, [PR #21](https://github.com/olincollege/noodlemaps/pull/21)), and each time prevented a merge before the bugs were fixed.

### End-to-End Tests

I also wrote the entirety of our [end-to-end tests](https://github.com/olincollege/noodlemaps/tree/bf964426354652664315b21305abe32925c79913/testing/end_to_end_tests), which ensures that the entire system works. Additionally, I [modified our existing GitHub action](https://github.com/olincollege/noodlemaps/blob/bf964426354652664315b21305abe32925c79913/.github/workflows/get-green-head.yml) to take into account these end-to-end tests, and prevent a merge into `green-head` should they fail. With these changes, any bugs that made it to `true-head` that prevented the app from running properly would be stopped before they could reach our main branch.
