# Nearsk submodule build problem test repo

This repository shows an error I am getting when I am trying to use Naersk to compile a rust crate which has a git submodule inside it.

To reproduce:
``` bash
# clone the repo
git clone https://github.com/molysgaard/naersk-issuereport.git

# go into the new repository
cd naersk-issuereport

# fetch the repos submodule
git submodule update --init --recursive

# try to build using naersk
nix build
```

Observe the following error:
```
user@hostname:~/naersk-issuereport$ nix build
error: builder for '/nix/store/llvlnxiia6x2zasq7i462wyk5m3dgq9c-parent-crate-deps-0.1.0.drv' failed with exit code 101;
       last 10 log lines:
       >
       > Caused by:
       >   Unable to update /build/dummy-src/itoa
       >
       > Caused by:
       >   failed to read `/build/dummy-src/itoa/Cargo.toml`
       >
       > Caused by:
       >   No such file or directory (os error 2)
       > [naersk] cargo returned with exit code 101, exiting
       For full logs, run 'nix log /nix/store/llvlnxiia6x2zasq7i462wyk5m3dgq9c-parent-crate-deps-0.1.0.drv'.
error: 1 dependencies of derivation '/nix/store/lrfy8vpac86qdcy9i2ma57mkh02457h2-parent-crate-0.1.0.drv' failed to build
```

On inspecting the `dummy-src/` directory, there is no `itoa` directory, implying that the submodule has not been fetched for some reason.
