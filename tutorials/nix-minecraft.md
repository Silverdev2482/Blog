## WARNING: MOSTLY UNCOMPLETED, WIP
## Modded Minecraft Fabric Server on NixOS Using [nix-minecraft](https://github.com/Infinidoge/nix-minecraft)

I use nix-minecraft to run my minecraft server, This should be a somewhat
simple guide but I ran into a few minor problems. for this guide I assume
you are using flakes, if you aren't using flakes you should just be able
to use a [template](../files/flake.nix). If you have problems please create
an issue so I can improve this guide, regardless if you managed to solve
them or not, I will do my best to help you, or find people that can,
just bother me before bothering someone like infinidoge.

### Flake requirements

As expected you should import the nix-minecraft flake in you inputs.
```nix
inputs.nix-minecraft.url = "github:Infinidoge/nix-minecraft";
```
Make sure to have the specialArgs and @inputs: stuff set up, I will
admit I don't know why they are needed but they are need. If you don't
know what I am talking about reffer to the [template](../files/flake.nix).
also don't forget to remove all imports in configuration.nix and transfer
them to the flake. This guide probably won't be the best and will be similar
to the official documentation but I think it will be slightly better.
Currently the tutorial is very WIP

### Minecraft server

This is the code snippet I use to run my minecraft server, and I will
explain it to you.
```nix
{ config, pkgs, lib, inputs, ... }:
{
  nixpkgs.overlays = [ inputs.nix-minecraft.overlay ];
  services.minecraft-servers = {
    enable = true;
    eula = true;
    dataDir = "/srv";
    runDir = "/srv";
    servers.survival =
      let
        modpack = pkgs.fetchPackwizModpack {
          url = "https://raw.githubusercontent.com/Silverdev2482/Survival-mods/main/pack.toml";
          packHash = "sha256-bnmc/pl20IBklV5Fjixh+GNKp1/D8cFhYDHPW0NYs1g=";
        };
        mcVersion = modpack.manifest.versions.minecraft;
        fabricVersion = modpack.manifest.versions.fabric;
        serverVersion = lib.replaceStrings [ "." ] [ "_" ] "fabric-${mcVersion}";
      in
      {
        enable = true;
        autoStart = true;
        jvmOpts = "-Xmx4096M -Xms4096M";
        openFirewall = true;
        package = pkgs.fabricServers.${serverVersion}.override { loaderVersion = fabricVersion; };
        symlinks = {
          "mods" = "${modpack}/mods";
        };
      };
 };
}
```
you can simply drop this in a file and import it in your flake, or put it
in your configuration.nix, just make sure everything that is in the top
part is present if you do that. 

the nixpkgs.overlays line does something to enable nix-minecraft.

The minecraft server will be store under $dataDir/$name, by default this
is /srv/minecraft, but since I don't use /srv for other stuff I prefer to
have my minecaft servers directly in it. enable and eula should be self
explanitory there is also the runDir which is like the dataDir but is for
a tmux socket, and where it is store, by default /run/minecraft. so you can
run tmux -S $SOCKET attach to connect to the console, you either need to be
root or part of the minecraft group to run this command. to exit tmux do
contol+B then D

enable and autostart are self explanatory however they are nice to change
compared to commenting evrything out.

jvmOpts may have other uses but the main thing you care about are the memory
settings, I reccomend you set it no higher than 75% of your memory amount,
and even that is risky on systems with <16gb of ram, so probably stick with
50% of your ram or less. to convert GiB to MiB multiply by 1024, 1000 works
fine but it feels wrong.

for the let statement and package reffer to Modpacks, for symlinks reffer to
config files.

### Ports, config files, and multiple servers


### Modpack

