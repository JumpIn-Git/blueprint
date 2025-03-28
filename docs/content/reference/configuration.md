# Configuration

The flake.nix file offers a few configuration options:

* **prefix**: This lets you specify a directory to hold the folders other than the flake.nix location.
* **systems**: Defines which systems the project should be used and deployed on.
* **nixpkgs.config**: If set, Blueprint will create a new instance of nixpkgs for each systems.
* **nixpkgs.overlays**: If set, blueprint will create a new instance of nixpkgs for each systems.

Below we provide more detail on each, along with examples.

[TODO: More detail and each; meanwhile I've copied in the current configuration.md file]

## prefix

Set this if you want to load the blueprint from a directory within the repositiry other than the flake location.

Default: "."

Type: string.

## systems

Defines for which systems the project should be used and deployed on.

Default: it will load the `inputs.systems` flake input, first from the current flake, and then fallback to the blueprint one. (see <https://github.com/nix-systems/default>).

Type: list of `<kernel>-<arch>` strings.

Example:

```nix
{
  outputs = inputs: inputs.blueprint {
    inherit inputs;
    systems = [ "aarch64-linux" "x86_64-linux" ];
  };
}
```

## nixpkgs.config

If set, blueprint will create a new instance of nixpkgs for each systems, with the passed config.

Default: `inputs.nixpkgs.legacyPackages.<system>`.

Type: attrset.

Example:

```nix
{
  outputs = inputs: inputs.blueprint {
    inherit inputs;
    nixpkgs.config.allowUnfree = true;
  };
}
```

## nixpkgs.overlays

> NOTE: It's better to use `perSystem` composition style instead of overlays if you can.

If set, blueprint will create a new instance of nixpkgs for each systems, with the passed config.

Default: `inputs.nixpkgs.legacyPackages.<system>`.

Type: list of functions.

Example:

```nix
{
  outputs = inputs: inputs.blueprint {
    inherit inputs;
    nixpkgs.overlays = [
      inputs.otherflake.overlays.default
      (final: prev: {
        git = final.gitMinimal;
      })
    ];
  };
}
```
