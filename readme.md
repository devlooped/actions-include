# +M▼ includes

A GitHub action that resolves includes (by default in markdown files) and 
embeds the included files.

Usage:

In your content files:

```html
<!-- include [RELATIVE_PATH][#ANCHOR] -->
```

The optional `#anchor` allows including fragments of files. Anchors are 
defined as:

```html
<!-- #ANCHOR -->
```

with an optional "closing" anchor defined exactly the same as the starting one.
If there is no closing anchor, the included content is from anchor declaration
to the end of the file. For example:

```html
<!-- #badges -->
[...] //some shields.io badges you use everywhere
<!-- #badges -->
```

Which can be included with:

```html
<!-- include common.md#badges -->
```


To run the action and automatically create a PR with the resolved includes:

```yml
on: 
  push:
    branches:
      - 'main'
    paths:
      - '**.md'    

jobs:
  includes:
    runs-on: ubuntu-latest
    steps:
      - name: 🤘 checkout
        uses: actions/checkout@v2

      - name: +M▼ includes
        uses: devlooped/actions-include@v1
        with: 
            include: '*.md'

      - name: ✍ pull request
        uses: peter-evans/create-pull-request@v3
        with:
          base: main
          branch: markdown-includes
          delete-branch: true
          commit-message: +M▼ includes
          title: +M▼ includes
          body: +M▼ includes
```

Note how you can trivially create PRs that update the changed 
files so the includes are always resolved automatically whenever 
you change any of the monitored files. The `include: '*.md'` 
above wouldn't be required since it's the default.

## How it works

You can include content from arbitrary external files with:

```Markdown
<!-- include header.md -->

# This my actual content

<!-- include footer.md -->
```

When the action runs for the first time, it will turn the 
above content into:

```Markdown
<!-- include header.md -->
This comes from the included header!
<!-- header.md -->

# This my actual content

<!-- include footer.md -->
This comes from the included footer!
<!-- footer.md -->
```

The action is idempotent, so it is safe for it to run on pushes of the 
same files it changed via the includes (since no further changes will 
be detected).

> NOTE: the included path must be relative to the including file. 

<!-- include docs/sponsors.md -->
[![Kirill Osenkov](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/KirillOsenkov.png "Kirill Osenkov")](https://github.com/KirillOsenkov)
[![C. Augusto Proiete](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/augustoproiete.png "C. Augusto Proiete")](https://github.com/augustoproiete)
[![SandRock](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/sandrock.png "SandRock")](https://github.com/sandrock)
[![Amazon Web Services](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/aws.png "Amazon Web Services")](https://github.com/aws)
[![Christian Findlay](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/MelbourneDeveloper.png "Christian Findlay")](https://github.com/MelbourneDeveloper)
[![Clarius Org](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/clarius.png "Clarius Org")](https://github.com/clarius)
[![MFB Technologies, Inc.](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/MFB-Technologies-Inc.png "MFB Technologies, Inc.")](https://github.com/MFB-Technologies-Inc)


<!-- docs/sponsors.md -->
