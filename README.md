# webr-package-repo-demo
Can we build and serve webr packages?


This demo includes an R package in the `packages` directory that is compiled to a Wasm package by the Github Action, then published to a CRAN style repo.

The package can be found at:

https://ouseful-testing.github.io/webr-package-repo-demo/src/contrib/bin/emscripten/contrib/4.4/PACKAGES.rds

Install as:

`install.packages("M348", repos="https://ouseful-testing.github.io/webr-package-repo-demo/src/contrib")`

Although this currently errors for me:

![image](https://github.com/user-attachments/assets/63119f58-4739-405c-976d-828ae4331297)

So we need to build the package for the correect version of R. The default version is set in `.github/workflows/test-package_build.yaml` eg as per`webr-image: "ghcr.io/r-wasm/webr:main"` which is currently at `"ghcr.io/r-wasm/webr:v0.4.1"`

We should be able to set the path directly in the calling action? eg in `.github/workflows/deploy-cran-repo.yml`:

```yaml
webr-image:
  description: Docker container image for webR development environment. Defaults to the latest version of webR.
  default: ghcr.io/r-wasm/webr:v0.3.3 #Original default: "ghcr.io/r-wasm/webr:main", currently at v0.4.1
```

For the jupyterlite kernel, we need `"ghcr.io/r-wasm/webr:v0.3.3"`

In use, when writing R code, the webR `library()` function appears to pre-emptively try to install a missing package from the webR repo. Setting `options(webr_pkg_repos= "https://ouseful-demos.github.io/jupyterlite-m348-demo/repo/")` should force install.packages() and the library() function to pull from this custom repo directly.

If we build our own webR release, we can presumably set that path via the `webr-image.default` value in the action YAML, although I'm not sure what URL the actual resource needs to be on?
