# Nix Flakes installer

This is a temporary place to host Nix Flakes releases, until the NixOS
project publishes official releases.

## Latest release

* Release: `nix-2.4pre20201110_cdc840d`
* Hydra eval: https://hydra.nixos.org/eval/1625859

## Usage

### Systems

```sh
sh <(curl -L https://github.com/newkozlukov/nix-flakes-installer/releases/download/nix-2.4pre20201110_cdc840d/install)
```

### GitHub Actions

```yaml
name: "Test"
on:
  pull_request:
  push:
jobs:
  tests:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
      with:
        # Nix Flakes doesn't work on shallow clones
        fetch-depth: 0
    - uses: cachix/install-nix-action@v11
      with:
        install_url: https://github.com/newkozlukov/nix-flakes-installer/releases/download//install
        # Configure Nix to enable flakes
        extra_nix_config: |
          experimental-features = nix-command flakes
    # Run the general flake checks
    - run: nix flake check
    # Verify that the main program builds
    - run: nix shell -c echo OK
```

## Current release process

* Go to https://hydra.nixos.org/jobset/nix/master
* Find the latest eval ID
* Run `./update.rb <eval ID>`
* Tag with the release ID.
* Push to GitHub
* Create a new GitHub release and attach all those files in the ./dist folder.
