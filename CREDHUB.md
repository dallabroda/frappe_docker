# Build da imagem do Cred Hub

Use **somente** `images/custom/Containerfile`. Ele parte da imagem oficial do Python (Debian trixie) e
não usa as imagens `frappe/base` e `frappe/build` do Docker Hub (tags móveis publicadas pela Frappe),
que o `images/layered/Containerfile` exige.

- Python, Node e bench fixos nas versões da produção (Python 3.14.9, Node 24.22.1, bench do `dallabroda/bench`).
- Framework vem de `FRAPPE_PATH` (padrão `https://github.com/dallabroda/frappe`) na branch/tag `FRAPPE_BRANCH`.
- Apps vêm do `apps.json` passado como secret `apps_json` (só repositórios `dallabroda/*`).

```bash
DOCKER_BUILDKIT=1 docker build --no-cache \
  --build-arg FRAPPE_BRANCH=<tag do dallabroda/frappe> \
  --secret id=apps_json,src=apps.json \
  -t credtech/credhub:<tag> -f images/custom/Containerfile .
```
