# webr-package-repo-demo
Can we build and serve webr packages?


This demo includes an R package in the `packages` directory that is compiled to a Wasm package by the Github Action, then published to a CRAN style repo.

The package can be found at:

https://ouseful-testing.github.io/webr-package-repo-demo/src/contrib/bin/emscripten/contrib/4.4/PACKAGES.rds

Install as:

`install.packages("M348", repos="https://ouseful-testing.github.io/webr-package-repo-demo/src/contrib")`

Although this currently errors for me:

![image](https://github.com/user-attachments/assets/63119f58-4739-405c-976d-828ae4331297)
