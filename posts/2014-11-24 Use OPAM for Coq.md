Coq is about to get a package manager: [OPAM](http://opam.ocamlpro.com/). This is already the package manager of [OCaml](https://ocaml.org/), so it seemed to be the natural choice for us, since Coq and its plugins are already coded in OCaml.

## Install OPAM
Install OPAM by your preferred method. It is recommended to use the latest version (`1.2` as of today), which can be installed from the sources:

    curl -L https://github.com/ocaml/opam/archive/1.2.0.tar.gz |tar -xz
    cd opam-1.2.0
    ./configure
    make lib-ext
    make
    sudo make install

By default the OPAM packages are installed in `~/.opam`. You can also have many installation folders. A good practice is to have one installation folder per project. Then your are safe if you need different versions of Coq or packages for each project. To configure OPAM for the current folder:

    mkdir opam
    opam init --root=opam # answer no to the question
    eval `opam config --root=opam env` # set the environment variables for this shell session

## Add the repositories
Now you can add the Coq [stable repository](https://github.com/coq/repo-stable):

    opam repo add coq-stable https://github.com/coq/repo-stable.git

The stable repository contains only released packages. There is also the [unstable repository](https://github.com/coq/repo-unstable) for development versions. You should use it at your own risks:

    opam repo add coq-unstable https://github.com/coq/repo-unstable.git

There is also the [coqs repository](https://github.com/coq/repo-coqs) with development versions of Coq:

    opam repo add coqs https://github.com/coq/repo-coqs.git

## Install a package
The Coq packages are in the namespace `coq:`. To list all of them:

    opam search coq:

To install a package:

    opam install coq:ssreflect

If the package is slow to install (for instance, Coq itself), you can always use the `--jobs=` option:

    opam install --jobs=6 coq

A website to list all the available packages, in the style of [opam.ocaml.org](http://opam.ocaml.org/), should be available soon.