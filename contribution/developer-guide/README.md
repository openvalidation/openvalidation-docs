# Developer guide

At this place you will find mainly the technical documentation for Contributors. If you would like to contribute to the development of **openVALIDATION**, then you have come to the right place.

If you would like to use **openVALIDATION** as a user or integrate the compiler into another project, please start at this point [Ready to go...](../../setup.md)

## How can I contribute?

### 1. Report Bugs and provide Feature Requests

We use GitHub issues for both. First, use the [github search](https://github.com/openvalidation/openvalidation/search?q=is%3Aissue&unscoped_q=is%3Aissue) to check if your issue has been reported already. If you find that someone already reported your problem, feel free to share additional information to help us investigate the bug more quickly.

If you could not find a matching issue and want to open a new ticket, please try to follow the issue template and provide all the necessary information. However, an incomplete ticket that reports a critical bug is still better than no ticket at all.

### 2. Become a developer

Please refer to the [developer readme](https://github.com/openvalidation/openvalidation/blob/master/docs/developer_readme.md) for build and test instructions, as well as our coding guidelines.

Pull requests are always welcome! If you have never contributed to an open source project on GitHub before, check out [this guide for first-timers](https://akrabat.com/the-beginners-guide-to-contributing-to-a-github-project/). When opening a new pull request, try to stick to the template as much as is reasonable, but don't be afraid to leave out or add information if that would make your PR more concise and readable.

### 3. Choose a good place to start

**Improving documentation**

Improving documentation \(code comments, wikis, website\) is an excellent way of familiarizing yourself with the project and improving the codebase at the same time!

**Fixing lint warnings**

We want to get openVALIDATION to be [checkstyle](https://checkstyle.org/) clean. In order to do this, open the [checkstyle configuration](https://github.com/openvalidation/openvalidation/blob/master/build-tools/src/main/resources/google_checks.xml) at `build-tools/src/main/resources/google_checks.xml` and re-add one of the commented-out lints.  
After having fixed all the warnings, submit a pull request.

**Fixing bugs**

Pick a ticket from the [bugtracker](https://github.com/openvalidation/openvalidation/issues) and leave a comment letting us know that you are working on a fix.  
If you have any questions, feel free to discuss it in the ticket. Make sure the fix adheres to the [coding guidelines](https://github.com/openvalidation/openvalidation/blob/master/docs/developer_readme.md).

**Implementing new features**

Before integrating a new feature, it's best to discuss it with the core developers first.  
This can be done in the corresponding GitHub issue in the [bugtracker](https://github.com/openvalidation/openvalidation/issues).

* Make sure you add tests of your functionality.
* All tests should pass, the code should be properly formatted \(see coding guidelines\) and continuous integration must give its ok on the pull request.
* Submit a [pull request](https://github.com/openvalidation/openvalidation/compare)

### 4. Stick to our license

All changes to this project must comply with the [Apache 2.0 License](https://github.com/openvalidation/openvalidation/blob/master/LICENSE.txt)

