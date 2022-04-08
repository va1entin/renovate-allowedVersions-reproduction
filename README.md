# renovate-allowedVersions-reproduction

In this repo I'm testing the [allowedVersions configuration](https://docs.renovatebot.com/configuration-options/#allowedversions) for [renovate](https://renovatebot.com).

## Initial problem 

TL;DR: Ignored dependency version does not result in new MRs containing just itself but still slips into MRs for other updates in the same repo.

Sometimes we have dependencies of which we use both the last and most recent major version at the same time in different places. An example would be Gitleaks: we use both version 7.6.1 (7.x is not being updated anymore) and the most recent 8.5.2 in the same (Gitlab CI) file.

Renovate used to always open MRs for these outdated versions, bumping for example Gitleaks 7.6.1 to 8.5.2. We kept closing them and eventually added a packageRule with negated allowedVersions to the repo to make renovate ignore a specific version of a specific dependency as documented here. 

Unfortunately though renovate still tries to update these versions when any other MR is created. So some totally different dependency is correctly updated but the corresponding MR also contains an update for the specific dependency/version combination that we have tried to make renovate ignore using the config above.

## Problem observed here

I couldn't reproduce the initial problem described above. In this repo, renovate doesn't respect allowedVersions at all it seems. It just creates/updates PRs for the negated version specified in allowedVersions, regardless of other updates.
