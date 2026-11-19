# Deploy to droply

Publish a folder from your repo to [droply](https://droply.so) on every push, and get a link like `https://my-docs.droply.page`.

Good for docs, reports, prototypes, AI-made pages and any static site: HTML, CSS, JavaScript, images, PDFs.

## Usage

1. In droply, open your dashboard → Account → Developer and create an API key. The API needs the Plus plan or higher ([pricing](https://droply.so/pricing)).
2. In your GitHub repo: Settings → Secrets and variables → Actions → New repository secret. Name: `DROPLY_API_KEY`.
3. Add `.github/workflows/droply.yml`:

```yaml
name: Publish to droply
on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v5

      # Build first if your site needs it, e.g.:
      # - uses: actions/setup-node@v5
      #   with: { node-version: 20 }
      # - run: npm ci && npm run build

      - uses: fcl11/deploy-to-droply@v1
        id: droply
        with:
          api-key: ${{ secrets.DROPLY_API_KEY }}
          name: my-docs
          path: ./dist

      - run: echo "Live at ${{ steps.droply.outputs.url }}"
```

The first run creates `my-docs.droply.page`. Later runs replace the files, and the link stays the same.

## Inputs

| Input | Required | Default | What it does |
|---|---|---|---|
| `api-key` | yes | | Your droply API key (`drp_...`). Keep it in a secret. |
| `name` | yes | | Project name, 3–40 lowercase letters, numbers and dashes. Becomes `<name>.droply.page`. |
| `path` | no | `.` | Folder to publish. Needs `index.html` at the top. Dotfiles and `node_modules` are skipped. |
| `password` | no | | Visitors must enter this password. Set when the project is first created. |

## Outputs

| Output | Example |
|---|---|
| `url` | `https://my-docs.droply.page` |
| `id` | the project id |

## Limits

- The API needs Plus or higher. Your plan's storage and project limits apply.
- Keep one deploy under about 100 MB.
- Everything you publish is public to anyone with the link, unless you set a password.

## Other ways to publish

- Command line and Claude Code skill: [fcl11/droply-cli](https://github.com/fcl11/droply-cli)
- REST API docs: [docs.droply.so](https://docs.droply.so)

## Licence

MIT. Questions: [droply.so/contact](https://droply.so/contact).
