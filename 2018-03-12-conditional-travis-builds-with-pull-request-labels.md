---
title: Conditional Travis Builds with Pull Request Labels
slug: conditional-travis-builds-with-pull-request-labels
date_published: 2018-03-12T20:20:53.000Z
date_updated: 2018-08-20T15:48:31.000Z
---

We have Travis configured to build all Github Pull Requests that will be merged into master.  This is very convenient, as all unit and end-to-end tests are run against the resulting merge, giving you an indication of how healthy the final build will be.

There is one big caveat with this - we like to open PRs as soon as we start work on feature branches, to get early feedback from other team members.  The upshot of this is that Travis would be running a build for every single commit during development of a feature.  Not great when you are limited to 5 concurrent builds, each one taking in excess of 40 minutes to complete.

Travis offers [conditional builds](https://docs.travis-ci.com/user/conditional-builds-stages-jobs/) but ideally we wanted a way to indicate whether a particular PR should be built or not.  Github allows you to label PRs, so i figured if we could somehow leverage the presence of a particular label into the Travis build, we would have a good way of letting Travis know which PRs to ignore, until they were feature complete and ready to be built.

I came up with the following script, which has no dependencies other than the Node core `https` module...

    'use strict'
    
    const https = require('https')
    const ciEnabledLabelId = 861719997
    // Example Github API Pull Request URL - 'https://github.com/DFEAGILEDEVOPS/MTC/pull/566'
    const pullRequestId = process.argv[2]
    
    if (!pullRequestId) {
      console.log('Missing argument: pull request id')
      process.exit(1)
    }
    
    const pullRequestUrl = `/repos/dfeagiledevops/mtc/pulls/${pullRequestId}`
    
    const options = {
      hostname: 'api.github.com',
      path: pullRequestUrl,
      method: 'GET',
      headers: {
        'User-Agent': 'node/https'
      }
    }
    
    const parseResponse = (res) => {
      let labels
      try {
        labels = JSON.parse(res).labels
        if (!labels || labels.length === 0) {
          console.log(`no labels found attached to PR ${pullRequestId}`)
          process.exit(1)
        }
      } catch (err) {
        console.error(`error parsing labels for PR ${pullRequestId}`)
        console.error(err)
        process.exit(1)
      }
      const ciEnabledLabel = labels.find(item => item.id === ciEnabledLabelId)
      if (ciEnabledLabel) {
        console.log(`CI enabled label found on PR ${pullRequestId}`)
        process.exit(0)
      }
      console.log(`CI Enabled label not found on PR ${pullRequestId}`)
      process.exit(1)
    }
    
    https.get(options, (response) => {
      let data = ''
    
      response.on('data', (chunk) => {
        data += chunk
      })
    
      response.on('end', () => {
        parseResponse(data)
      })
    }).on('error', (err) => {
      console.error('Error: ' + err.message)
    })
    

This can then be invoked at the very first stage of a Travis build `before install` by passing the script the (rather convenient) `$TRAVIS_PULL_REQUEST` variable, as follows...

    before_install:
      - node ./deploy/should-it-build.js $TRAVIS_PULL_REQUEST
    

If the PR being built by Travis does not contain the label with the Id matching the value of the variable `ciEnabledLabelId`, it will return a non-zero exit code and fail the build.

There are 2 pieces of information you will need to modify for this to work...

- **ciEnabledLabelId** This is the numeric identifier of the label within your Github repo that you are using to indicate whether the build should proceed or not.  You can easily find this by adding the label to a PR and reading the JSON you receive when issuing a GET request to the Github API for the PR.
- **pullRequestUrl** The path to *your* Github repo
