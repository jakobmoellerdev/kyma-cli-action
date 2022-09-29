# kyma-cli-action
A Github Action for Kyma CLI

Sample Usage:
```yaml
# This is a basic workflow to help you get started with Actions

name: CI

# Controls when the workflow will run
on:
  # Triggers the workflow on push or pull request events but only for the "main" branch
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# A workflow run is made up of one or more jobs that can run sequentially or in parallel
jobs:
  # This workflow contains a single job called "build"
  build:
    # The type of runner that the job will run on
    runs-on: ubuntu-latest

    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:
      # outputs version by default
      - id: kyma-stable
        uses: jakobmoellersap/kyma-cli-action@main
     
      # you can use any command here, but remember to setup prerequisites e.g. k3d first
      - run: wget -qO - https://raw.githubusercontent.com/k3d-io/k3d/main/install.sh | TAG=v5.4.6 bash
      - id: kyma-stable
        uses: jakobmoellersap/kyma-cli-action@main
        with:
          command: provision k3d -p 8083:80@loadbalancer -p 8443:443@loadbalancer

      - id: kyma-unstable
        uses: jakobmoellersap/kyma-cli-action@main
        with:
          version: unstable
          
      - id: kyma-271
        uses: jakobmoellersap/kyma-cli-action@main
        with:
          version: 2.7.1
      
      - id: kyma-main
        uses: jakobmoellersap/kyma-cli-action@main
        with:
          version: main
          
      - name: Run Versions from previously downloaded bins
        run: |
          ${{ steps.kyma-stable.outputs.path }} version --client
          ${{ steps.kyma-unstable.outputs.path }} version --client
          ${{ steps.kyma-271.outputs.path }} version --client
          ${{ steps.kyma-main.outputs.path }} version --client

```
