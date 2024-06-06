---
Title: Getting started with NixOS
Date: 2024-04-21
Modified: 2024-06-06
Tags: linux, nixos
slug: nixos-beginnings
Summary: With the impending threat of Windows 10 EOL, I want to go back to daily driving Linux.
---

## NixOS

> Update: After facing several issues while trying to install some packages from the stable branch and others from the unstable branch of nix-pkgs, I've switched to Fedora. After a couple weeks with KDE, I've also started using i3, and it's been good so far.

After having some keyboard issues with several install attempts in a row of several versions of Ubuntu, I gave up on it. I decided that it was finally time for me to try a different distro, and I've heard a lot about NixOS recently, so I figured I'd give it a try. There was only two small hiccups that I ran into:

- The graphical NixOS installer doesn't have a page for connecting to a network. Some googling led me to `nmtui`, which I was able to use to connect to my wifi network and go through the installation process.
- Something went wrong with the grub setup, I can only assume related to directly trying to replace Ubuntu with NixOS. I ended up going back into Windows to completely de-partition the SSD, then let the NixOS installer reformat the entire drive for grub + NixOS.

## NixOS Concepts

The whole point of Nix (and NixOS) as I understand it is to make the entire configuation of your computer to be declarative, meaning it's all in config files that you know (and control) where they are, down to installing packages. There's a lot more to Nix that hasn't clicked for me yet, but [Zero to Nix](https://zero-to-nix.com/) was helpful for getting the important parts without digging through the entire manual.

## Config with flakes

One important part of Nix that hasn't quite clicked for me yet is flakes. The one thing I've grasped is that it's an addition to the Nix paradigm that replaces channels, and improves reproduceability by creating a companion `flake.lock` file that records specific versions, similar to `cargo.lock` for rust or `requirements.txt` with python. I also know that by putting my entire system config into a flake, I can move the main configuration file out of `/etc/nixos/configuration.nix` to wherever I want. I got a jump start here using [Misterio77's nix-starter-configs](https://github.com/Misterio77/nix-starter-configs). This also brings up home manager, which I have learned is about bringing traditional .config files into your nix config, but have not actually figured out yet.

The starter config has left me with:
```
~/nix-config/
├── flake.lock
├── flake.nix
├── home-manager
│   └── home.nix
└── nixos
    ├── configuration.nix
    └── hardware-configuration.nix
```

Which has changed my rebuild command from:
```shell
sudo nixos-rebuild switch
```
To: (executed from ~/nix-config)
```shell
sudo nixos-rebuild switch --flake .
```

## First Configuration: Key remapping

I have grown to absolutely love having Caps Lock and Escape swapped, not just while using vim/neovim, but in general it feels like I use escape way more often, so it's really helpful to move it closer. After fiddling around with several different options, I got [Xremap](https://github.com/xremap/xremap) working thanks to some wonderful videos by 
Vimjoyer ([main video](https://www.youtube.com/watch?v=lyxScRCe6bE), [NixOS companion video](https://www.youtube.com/watch?v=UPWkQ3LUDOU)). 

The changes I had to make were as follows:
- In my `flake.nix`, add the link to xremap as an input
  ```nix
  inputs = {
    # ...
    xremap-flake.url = "github:xremap/nix-flake";
  };
  ```
- In my `configuration.nix`, include xremap as an import
  ```nix
  imports =
    [
      # ...
      inputs.xremap-flake.nixosModules.default
    ];
  ```
- In my `configuration.nix`, enable and configure xremap
  ```nix
  services.xremap = {
    userName = "my_username";
    config = {
      keymap = [
        {
          name = "main remaps";
          remap = {
            "capslock" = "esc";
            "esc" = "capslock";
          };
        }
      ];
    };
  };
  ```
Then just rebuild nixos, and voila, my wonderful remap is here.

## Windows Dual boot

The correct lines for the configuration file to get grub to find a list windows were:
```nix
boot.loader = {
  efi.canTouchEfiVariables = true;
  grub = {
    enable = true;
    device = "nodev";
    efiSupport = true;
    useOSProber = true;
  };
};
```
