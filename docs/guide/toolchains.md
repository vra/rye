# Toolchain Management

Rye is unique in that it does not use system Python installations.  Instead if downloads
and manages Python installations itself (called toolchains).

## Pinning Toolchains

To make a project use a specific toolchain write the name of the toolchain into the
`.python-version` file or use the `pin` command.  For pinning `cpython` the `cpython@`
prefix can be omitted.

```
rye pin cpython@3.11.4
```

Pinning a downloadable version means that Rye will automatically fetch it when necessary.

## Listing Toolchains

To see which toolchains are installed, `rye toolchain list` prints a list:

```
rye toolchain list
```
```
cpython@3.11.1 (C:\Users\armin\.rye\py\cpython@3.11.1\install\python.exe)
pypy@3.9.16 (C:\Users\armin\.rye\py\pypy@3.9.16\python.exe)
```

To see which toolchains can be installed, additionally pass the `--include-downloadable`:

```
rye toolchain list --include-downloadable
```

## Fetching Toolchains

Generally Rye automatically downloads toolchains, but they can be explicitly fetched
with `rye toolchain fetch` (also aliased to `rye fetch`):

```
rye toolchain fetch cpython@3.8.5
```

Toolchains are fetched from two sources:

* [Indygreg's Portable Python Builds](https://github.com/indygreg/python-build-standalone) for CPython
* [PyPy.org](https://www.pypy.org/) for PyPy

## Registering Toolchains

Additionally it's possible to register an external toolchain with the `rye toolchain register`
command.

```
rye toolchain register /path/to/python
```

The name of the toolchain is picked based on the interpreter.  For instance linking a regular
cpython installation will be called `cpython@version`, whereas linking pypy would show up as
`pypy@version`.  To override the name you can pass `--name`:

```
rye toolchain register --name=custom /path/to/python
```

## Removing Toolchains

To remove an already fetched toolchain run `rye toolchain remove`.  Note that this
also works for linked toolchains:

```
rye toolchain remove cpython@3.8.5
```

!!! Warning

    Removing an actively used toolchain will render the virtualenvs that refer to use broken.
