# webr-package-repo-demo

Can we build and serve webr packages?

This demo includes several R packages used in the OU module M348 in the `packages` directory that are compiled to Wasm packages by the Github Action, then published to a CRAN style repo.

## Adding and Building New Packages

Add references to packages that need to be built to the `.github/workflows/test-package_build.yml` file:

- if package source is provided (`.tar.gz`), put the `.tar.gz` source file in `./packages` and refer to as `local::./packages/PACKAGE.tar.gz` in the `.yml` file
- for packages on CRAN, just give the package name (e.g. `jsonlite`)

In the `.github/workflows/test-package_build.yml` file:, specify the R version via the `webr-image:` parameter (eg `webr-image:"ghcr.io/r-wasm/webr:v0.3.3"`).

Build using the __Build M348 deployment packages__ action. The files are published as a repo via GitHub pages and also uploaded as an artefact to the action run report page (the artefact download link expires after a certain number of days.)

## Accessing the Repo

The packages manifest can be found at:

[https://ouseful-testing.github.io/webr-package-repo-demo/src/bin/emscripten/contrib/4.3/PACKAGES.rds](https://ouseful-testing.github.io/webr-package-repo-demo/src/bin/emscripten/contrib/4.3/PACKAGES.rds) (link downlads RDS file)

Packages can then be installed as:

`install.packages("M348", repos="https://ouseful-testing.github.io/webr-package-repo-demo/src/contrib")`

If there is a JupyterLite `webr` kernel mismatch, an error will be generated:

![image](https://github.com/user-attachments/assets/63119f58-4739-405c-976d-828ae4331297)

So we need to build the package for the correct version of R. The default version is set in `.github/workflows/test-package_build.yaml` eg as per `webr-image: "ghcr.io/r-wasm/webr:main"` which is currently at `"ghcr.io/r-wasm/webr:v0.4.1"`; to build for the 4.3 kernel used by the current webr jupyterlite kernel, we need to set: `webr-image: "ghcr.io/r-wasm/webr:v0.3.3"`

We scan set the path directly in the calling action, e.g. in `.github/workflows/deploy-cran-repo.yml`:

```yaml
webr-image:
  description: Docker container image for webR development environment. Defaults to the latest version of webR.
  default: ghcr.io/r-wasm/webr:v0.3.3 #Original default: "ghcr.io/r-wasm/webr:main", currently at v0.4.1
```

For the current jupyterlite kernel, we need `"ghcr.io/r-wasm/webr:v0.3.3"`

In use, when writing R code, the webR `library()` function appears to pre-emptively try to install a missing package from the webR repo.

Setting `options(webr_pkg_repos= "https://github.com/ouseful-testing/webr-package-repo-demo")` should force `install.packages()` and the `library()` function to pull packages from the package repository published using GitHub Pages from this repository.

If we copy the repository files into a `/repo` directory in a JupyterLite deployment site (eg rooted on `https://ouseful-demos.github.io/jupyterlite-m348-demo/`), we can set `options(webr_pkg_repos= "https://ouseful-demos.github.io/jupyterlite-m348-demo/repo/")` and then the packages will be pulled from the same domain as JupyterLite distribution.

If we build our own webR release, we can presumably set that path via the `webr-image.default` value in the action YAML, although I'm not sure what URL the actual resource needs to be on?
