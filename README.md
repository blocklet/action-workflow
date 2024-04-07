# Action Workflow
GitHub action for developing blocklet workflow

Example workflow

```yml
on:
  push:
    # Sequence of patterns matched against refs/tags
    tags:
      - 'v*' # Push events to matching v*, i.e. v1.0, v20.15.10

name: Blocklet action workflow

jobs:
  Deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2
      - name: Build
        run: <build_your_blocklet> # after build, use `abtnode bundle --create-release` to bundle your blocklet
      - name: Blocklet Action Workflow
        uses: blocklet/action-workflow@v0.2.0
        with:
          # prepare step
          # skip-deps: true  # default is false, set true to skip prepare @blocklet/cli step  Alias: skip-abtnode

          # bundle step
          # skip-bundle: true  # default is false, set true to skip bundle blocklet step
          bundle-command: yarn build && blocklet bundle --create-release

          # upload step
          # skip-upload: false  # default is true, skip upload blocklet step
          store-endpoint: ${{ secrets.STAGING_REGISTRY }} # Alias: registry-endpoint
          store-access-token: ${{ secrets.ABTNODE_DEV_STAGING_SK }} # Alias: developer-sk
          store-developer-did: 'xxxxxxxxxx' # Specify a developer's did as uploader

          # deploy step
          # skip-deploy: false  # default is true, skip deploy blocklet to abtnode step
          server-endpoint: ${{ secrets.STAGING_NODE_ENDPOINT }} # Alias: abtnode-endpoint
          server-access-key: ${{ secrets.STAGING_NODE_ACCESS_KEY }} # Alias: access-key
          server-access-secret: ${{ secrets.STAGING_NODE_ACCESS_SECRET }} # Alias: access-secret
          deploy-app-did: 'xxxxxxxxxx' # deploy as another blocklet's component blocklet
          deploy-mount-point: '/parent-path' # If is a component blocklet, need setup a mount point for component blocklet
          slack-webhook: ${{ secrets.SLACK_WEBHOOK }} # optional, if empty, will not send slack notification

          # release step
          # skip-release: true  # default is false, set true to skip release blocklet to github
          # skip-readme: true  # default is false, set true to skip update readme
          github-token: ${{ secrets.GITHUB_TOKEN }}
```

View complete document is here: [Blocklet Actions](https://www.arcblock.io/docs/blocklet-developer/en/blocklet-actions)
