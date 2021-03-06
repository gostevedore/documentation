# stevedore documentation

This repository holds the Stevedore documentation source code. To visit the site click [here](https://gostevedore.github.io/documentation)

The website is built using [Hugo](https://gohugo.io/), as static website generator, and [Geekdocs](https://geekdocs.de/) theme.

## Deployment process
This repository provides a github action named `Publish` which manages the deployment.
When a new documentation release is ready to be deployed on your development environment you must follow the steps below to perform the deployment:

1) Create a pull request to `main` branch
2) Merge the pull request, an then the `Publish` action is triggered.
3) Once the `Publish` action is completed properly, the site is already available on https://gostevedore.github.io/documentation
> The `Publish` action pushes the code generated by `Hugo` to `gh-pages` repository's branch.