# Building this Project

## Prerequisite

* Linux - Documentation is run routinely by our CI
* Windows - Developed with [Windows Subsytem for Linux (WSL)](https://docs.microsoft.com/en-us/windows/wsl/install-win10)
* Doxygen - [Doxygen](http://www.doxygen.nl/) is used to generate the api documentation for the C SDK and is
  necessary to be on the path

## Building the Doc

Github offers [github pages](https://pages.github.com/) which this project uses to host the output of building the
static content. Github has a few options for where you can put your doc at this time, the main branch, a folder on the
main branch named 'docs' or a special branch that still works named "gh-pages". This project is currently configured
to use the main branch and docs folder.

The best/easiest thing to do in order to build these docs is to have Windows Subsytem for Linux installed or any shell
which can execute a `.sh` script. As of 2020 there's a multitude of ways to get a bash/shell interpreter in windows.
It's not feasible to test all these shells to make sure this script works so it's encouraged that you use a linux-based
flavor of bash. If the script doesn't funtion - open an [issue](./issues) and someone will look into it.

After cloning this repository open the bash shell and execute the [gendoc.sh](./gendoc.sh) script. The script has a few
flags to pass that mostly controls the cleanup of what the script does. In general, it's recommended you use the -w flag
so that warngings are treated as errors. 

Expected gendoc.sh usage: `./gendoc.sh -w`

You can then run `cd ./docusaurus && yarn start` to serve the Docusaurus site from webpack. If you're testing configuration changes you will need to serve the production build with `yarn serve` instead.

## Publish by Running CI Equivalent Locally

`./publish.sh` is intended to run in GitHub Actions on branch `main` where the following variables are defined:

* `GIT_BRANCH`: output of `git rev-parse --abbrev-ref HEAD`
* `gh_ci_key`: base64 encoding of an OpenSSH private key authorized to clobber the `master` branch of [the root doc repo for GitHub pages](https://github.com/openziti/openziti.github.io/tree/master).

## How Search Works

Algolia [DocSearch](https://docsearch.algolia.com/) provides search for this site.

* Docusaurus v2's classic preset theme provides an integration with Algolia DocSearch v3 as a built-in plugin. The public search API key and application ID are properties in `docusaurus.config.js`. That plugin provides [the site's `/search` URL](/search) and a search widget in the main navigation ribbon that's visible on all pages. The Javascript running in these elements returns search results from the DocSearch API.

* The DocSearch API fetches records from an Algolia index that is populated by [an Algolia crawler](https://crawler.algolia.com/). Search results may be tuned by adjusting the crawler config. The crawler is specifically configured for the Docusaurus theme. If the theme changes structurally then it will be necessary to adjust the crawler config to suit. Changes to the site will not be reflected in the search results if the crawler config does not match the theme. The current config is based on [this configuration template for Docusaurus v2](https://docsearch.algolia.com/docs/templates/#docusaurus-v2-template).
