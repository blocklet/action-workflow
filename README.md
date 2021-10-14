# action-workflow

Github action for develop blocklet workflow

Example workflow

```yml
on:
  push:
    # Sequence of patterns matched against refs/tags
    tags:
      - 'v*' # Push events to matching v*, i.e. v1.0, v20.15.10

name: Blocklet workflow

jobs:
  Deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2
      - name: Build
        run: <build_your_blocklet> # after build, use `abtnode bundle --create-release` to bundle your blocklet
      - name: Release to Github
        uses: blocklet/action-workflow@v0.2.0
        with:
          # skip-abtnode: true  # default is false, set true to skip prepare abtnode step
          # bundle
          # skip-bundle: true  # default is false, set true to skip bundle blocklet step
          bundle-command: yarn build && blocklet bundle --create-release
          # upload
          # skip-upload: false  # default is true, skip upload blocklet step
          registry-endpoint: ${{ secrets.STAGING_REGISTRY }}
          developer-sk: ${{ secrets.ABTNODE_DEV_STAGING_SK }} # or replace with access-token: ${{ secrets.ACCESS_TOKEN }}  # (new abtnode upload flow)
          # deploy
          # skip-deploy: false  # default is true, skip deploy blocklet to abtnode step
          abtnode-endpoint: ${{ secrets.STAGING_NODE_ENDPOINT }}
          access-key: ${{ secrets.STAGING_NODE_ACCESS_KEY }}
          access-secret: ${{ secrets.STAGING_NODE_ACCESS_SECRET }}
          slack-webhook: ${{ secrets.SLACK_WEBHOOK }} # optional, if empty, will not send slack notification
          # release
          # skip-release: true  # default is false, set true to skip release blocklet to github
          github_token: ${{ secrets.GITHUB_TOKEN }}
```
