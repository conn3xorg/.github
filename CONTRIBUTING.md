# Contributing to a Conn3x circuit

Thanks for stopping by. Each `circuit-*` repo is an OpenTofu module with
Conn3x circuits inside, and most of them are small enough to read in one
sitting. That makes them a good place to start: a missing input, a distro
quirk, a README that got something wrong.

Bug reports help just as much as code. So does telling us a circuit worked
for you somewhere we haven't tried.

## Where to ask

Questions, ideas and "would you take a PR for this?" all go to
[Conn3x Discussions](https://github.com/conn3xorg/conn3x/discussions).
There's one place for every circuit, so nothing gets lost in a quiet repo.

For anything bigger than a small fix, open an issue or a discussion first.
It's much easier to agree on an approach before the code exists.

If you're looking for somewhere to begin, issues labelled
[`good first issue`](https://github.com/search?q=org%3Aconn3xorg+label%3A%22good+first+issue%22+state%3Aopen&type=issues)
are small and well scoped, and
[`help wanted`](https://github.com/search?q=org%3Aconn3xorg+label%3A%22help+wanted%22+state%3Aopen&type=issues)
marks the ones we'd especially like a hand with. Say on the issue that
you're taking it, so nobody else starts the same work.

Changes to Conn3x itself, the CLI and the engine, belong in
[conn3xorg/conn3x](https://github.com/conn3xorg/conn3x). Its
[contributing guide](https://github.com/conn3xorg/conn3x/blob/main/CONTRIBUTING.md)
covers the Go side.

## What you need

- [Conn3x](https://github.com/conn3xorg/conn3x/releases), at the version the
  circuit's README asks for under **Requirements**
- [Podman](https://podman.io/), because the test hosts run systemd
- An SSH client

## Try your change against a real host

Most circuits have an `examples/local` folder. Its `up.sh` starts a
throwaway Rocky Linux 10 container with systemd and SSH on
`127.0.0.1:2222`, and that's the host the example configures:

```sh
cd examples/local
./up.sh
conn3x init
conn3x apply
conn3x plan
```

The second plan has to say `No changes.` If it doesn't, something in the
circuit isn't idempotent yet, and that's a bug worth fixing before the PR.

Some examples need a variable first, usually a password. The example's own
README says which one, for instance
`export TF_VAR_app_password='pick-one'`.

When you're done, `./down.sh` removes the container, its volumes, the SSH
key and the state.

### The four distros

CI runs that same example on four distros, and a PR can't merge until all
four pass:

- Rocky Linux 10
- Fedora 45
- Ubuntu 24.04
- Debian 13

You can run them yourself. The images live in Conn3x's
[`test/images`](https://github.com/conn3xorg/conn3x/tree/main/test/images),
and `up.sh` takes any of them through `CONTAINERFILE`:

```sh
git clone --depth 1 https://github.com/conn3xorg/conn3x.git ~/conn3x
CONTAINERFILE=~/conn3x/test/images/debian13/Containerfile ./up.sh
```

Swap `debian13` for `rocky10`, `fedora45` or `ubuntu2404`. Run `./down.sh`
between distros. Testing the one you changed something for is fine; CI
covers the rest.

A few circuits, such as `circuit-proxmox` and `circuit-microshift`, need a
real kernel and can't run in a container. CI only validates those, and their
README's **Tested on** section says how they were tested by hand. If you
change one, tell us in the PR what you ran it on.

## Before you open a pull request

- **Format.** Run `conn3x fmt -recursive`. CI runs
  `conn3x fmt -check -recursive` and fails on any difference.
- **Validate.** CI runs `conn3x init -backend=false && conn3x validate` in
  the repo root and in each example. You can run the same thing.
- **Keep the README in step.** The Inputs and Outputs tables have to match
  `variables.tf` and `outputs.tf`: every variable and output, with the same
  name, type and default. If you add, rename or change one, update its row.
- **Version bumps come with checksums.** Where a circuit downloads a release
  and pins its SHA-256 (see `checksums` in `circuit-qdrant`), bumping the
  version means updating the checksums in the same change, from the
  project's own release page. Update the defaults the README shows, too.

## Commits

Sign off every commit with the
[Developer Certificate of Origin](https://developercertificate.org/):

```sh
git commit -s -m "fix: restart the service when the config changes"
```

The sign-off says you wrote the change, or otherwise have the right to
submit it under the repo's license. There's no CLA, and you keep the
copyright to your work. If you forgot, `git commit --amend -s` fixes the
last commit, and `git rebase --signoff main` fixes all of them.

Commit messages follow [Conventional Commits](https://www.conventionalcommits.org/):
`feat:`, `fix:`, `docs:`, `test:`, `refactor:`, `ci:`, `chore:`. Keep the
subject under 72 characters, and say what the problem was rather than only
what you did.

## Pull requests

- Branch from `main` and open the PR against `main`. Nobody pushes to `main`
  directly, maintainers included.
- One change per PR. Two unrelated fixes are two PRs.
- CI has to pass: `validate`, plus the four `e2e` jobs where the circuit
  has them.
- PRs are squash-merged, so the PR title becomes the commit subject.

We aim to reply within a few days. If we ask for a change, there's no rush.
The [Conn3x guide](https://github.com/conn3xorg/conn3x/blob/main/CONTRIBUTING.md#when-a-pull-request-goes-quiet)
explains what happens if a PR goes quiet; circuits work the same way.

When your PR merges, we thank you by name.

## Conduct and security

Everyone taking part follows the
[code of conduct](https://github.com/conn3xorg/.github/blob/main/CODE_OF_CONDUCT.md).
If you've found a security problem, please don't open an issue. The
[security policy](https://github.com/conn3xorg/.github/blob/main/SECURITY.md)
says how to report it privately.

## License

Each circuit is licensed under the Mozilla Public License 2.0. By
contributing, you agree that your contribution is licensed under it too.
