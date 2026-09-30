---
coverY: 0
---

# Contributing to pyspi

We deeply appreciate your interest in contributing to _pyspi_. Contributions play a vital role in the continual development and enhancement of _pyspi_, a project built and enriched through community collaboration. By participating in this project, you are contributing to the broader community and helping shape the future of this software.

Code is not the only way to contribute to _pyspi_. Reviewing pull requests, answering questions to help others and aid in troubleshooting, organising and teaching tutorials, improving documentation are all priceless contributions to the project.&#x20;

## Our Code of Conduct

_pyspi_ is committed to fostering a welcoming and inclusive community. All contributors and participants are expected to adhere to our Code of Conduct. Before engaging with the _pyspi_ project, please take some time to read our [_Code of Conduct_](code-of-conduct.md) and ensure that you abide by its terms. We believe in a respectful and harassment-free experience for everyone, and our Code of Conduct outlines our expectations for participant behaviour.&#x20;

## Types of contributions we seek

_pyspi_ thrives on a wide range of contributions, and we welcome your ideas, feedback and code. Here are some of the ways you can contribute to the project:

* **Integrating new SPIs**: We strive to provide the most comprehensive toolkit of SPIs. If you have ideas for new SPIs or improvements to existing ones, we are eager to hear and collaborate with you. Any pairwise dependence measure, provided it is accompanied by a published research paper, typically falls within our scope for consideration in the _pyspi_ library. For an in-depth guide on how you can integrate new SPIs into _pyspi_, see [here](incorporating-new-spis.md).&#x20;
* **Bug fixes**: Identifying and fixing bug are critical to maintaining the quality and reliability of _pyspi_. We use GitHub issues to track all bugs an feature requests. Feel free to open an issue if you have found a bug or wish to see a feature implemented.  &#x20;
* **Documentation**: Enhancing and updating our documentation to make it more comprehensive, up-to-date and user-friendly. If you find a typo in the documentation, or have suggestions for improvements, do not hesitate to send an email to the mailing list or preferably submit a GitHub pull request.
* **Examples and tutorials**: Creating practical and informative examples or tutorials to assist users in understanding and utilising _pyspi_.&#x20;
* **Troubleshooting:** Another way to contribute is to report issues you are facing and give a "thumbs up" on issues that others reported and that are relevant to you. It also helps us if you spread the word by referencing the project from your blogs and articles, link to it from your website, or simply 'star' the project on GitHub to say "I use it".&#x20;

## Bug Reporting Guidelines

We value bug reports as they play a crucial role in improving the quality of _pyspi_. We use GitHub issues to track bug requests, so if you encounter issues using this package, do not hesitate to submit a ticket to the [Bug Tracker](https://github.com/DynamicsAndNeuralSystems/pyspi/issues).

Before submitting a ticket, it is recommended to check that your issue complies with the following rules before submitting:

* Verify that your issue is not currently being addressed by an [existing ticket](https://github.com/DynamicsAndNeuralSystems/pyspi/issues).
* Ensure that your bug report complies with the _pyspi_ bug report guidelines.

### Submitting an ideal bug report

When submitting a bug report to the issue tracker in GitHub, please ensure that you follow these guidelines. This will streamline the resolution process and allow community members to provide good feedback. A good bug report shouldn’t leave others needing to chase you up for more information. Please try to be as detailed as possible in your report:

* The ideal bug report should contain a **short reproducible code snippet**, this way anyone can try to reproduce the bug easily. If your snippet is longer than 50 lines, please link to a gist or GitHub repo.&#x20;
* If not feasible to include a reproducible snippet, please be specific about what classes/functions involved.
* If an exception is raised, please **provide the full traceback**.&#x20;
* Include your **operating system, architecture** and **version number**, as well as your Python **version**.&#x20;
* Ensure all code snippets and error messages are formatted in [appropriate code blocks](https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/creating-and-highlighting-code-blocks).&#x20;

***

## Pull Request Guidelines

To ensure a smooth and efficient review process, we ask that you adhere to the following guidelines when submitting a pull request:

* **Include tests:** If your changes include new features or modifications to existing ones, please include appropriate tests. Well-written tests are fundamental to ensuring the reliability and stability of _pyspi_. To get an idea of the structure and conventions for writing unit tests, see the [source code](https://github.com/DynamicsAndNeuralSystems/pyspi/tree/main/tests) for existing _pyspi_ unit tests. Note we use [_**pytest**_ ](https://docs.pytest.org/en/7.4.x/)for all unit testing as standard.&#x20;
* **Update documentation**: If your changes involve user-facing features or significant modifications to existing functionalities, please update the relevant documentation sections.
* **Follow Code Standards:** Ensure your code adheres to the coding standards and style guidelines of _pyspi_. Consistent coding styles contribute to the readability and maintainability of the project.
* **Compatibility checks**: Verify that your changes are compatible with the supported versions of Python and any other dependencies. All tests should pass on the continuous integration (CI) system after your pull request is submitted.

Submitted pull requests are reviewed on a regular basis. The _pyspi_ core team evaluates each contribution for its quality, relevance, and alignment with the project's goals. Feedback will be provided, and we expect contributors to engage in a collaborative review process. If a pull request remains inactive for an extended period after feedback has been provided, it may be closed to keep the project's backlog manageable.&#x20;

***
