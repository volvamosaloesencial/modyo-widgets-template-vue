# Widget Vue.js Template

Readme File

```yml
name: Build and Publish
on:
  push:
    branches:
      - master
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - name: Setup Node.js 12.x
      uses: actions/setup-node@v1
      with:
        node-version: ${{ matrix.node-version }}
        registry-url: https://npm.pkg.github.com/
        scope: '@modyo'
    - name: Install dependencies with yarn
      run: yarn
      env:
        NODE_AUTH_TOKEN: ${{ secrets.TOKEN_REG }}
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }} # This gets generated automatically
    - name: Build Package
      run: yarn build
    - name: Push to Modyo Site
      run: yarn modyo-push "$MODYO_WIDGET_NAME"
      env:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }} # This gets generated automatically
        MODYO_ACCOUNT_URL: ${{secrets.BASE_URL}}
        MODYO_VERSION: ${{secrets.VERSION}}
        MODYO_TOKEN: ${{secrets.TOKEN}}
        MODYO_SITE_ID: ${{secrets.SITE_ID}}
        MODYO_WIDGET_NAME: ${{secrets.WIDGET_NAME}}
    - name: Push to Spanish Modyo Site
      run: yarn modyo-push "$MODYO_WIDGET_NAME"
      env:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }} # This gets generated automatically
        MODYO_ACCOUNT_URL: ${{secrets.BASE_URL}}
        MODYO_VERSION: ${{secrets.VERSION}}
        MODYO_TOKEN: ${{secrets.TOKEN}}
        MODYO_SITE_ID: ${{secrets.SITE_ID_ES}}
        MODYO_WIDGET_NAME: ${{secrets.WIDGET_NAME}}
    - name: Release Draft
      uses: release-drafter/release-drafter@v5
      with:
        # (Optional) specify config name to use, relative to .github/. Default: release-drafter.yml
        config-name: release-drafter.yml
      env:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```
