# ❄️ Nix macOS Cache (Intel / x86_64)

[![Build and Cache Nix Packages](https://github.com/sosumappu/my-nix-cache/actions/workflows/build-cache.yml/badge.svg)](https://github.com/sosumappu/my-nix-cache/actions/workflows/build-cache.yml)
[![Cachix Cache](https://img.shields.io/badge/Cachix-Cache-blue.svg)](https://app.cachix.org/)
[![Nix](https://img.shields.io/badge/Nix-Reproducible-5277C3.svg?logo=nixos&logoColor=white)](https://nixos.org/)

A personal binary cache powered by GitHub Actions to save aging Intel Macs from melting down while compiling heavy Nix packages.

## 🎯 The Problem

The official Nix build farm (Hydra) prioritizes Linux and Apple Silicon (`aarch64-darwin`). As a result, older Intel Macs (`x86_64-darwin`) often experience "cache misses" for complex packages like Deno, Direnv, or Neovim, forcing the local machine to compile them from source.

## 💡 The Solution

This repository uses GitHub Actions' macOS Intel runners (`macos-13`) to automatically fetch the latest `nixpkgs-unstable`, compile specifically targeted packages, and push the resulting binaries to [Cachix](https://cachix.org/).

When a local Intel Mac updates its flake lock, it will ping the Cachix server, find the pre-compiled `.nar` file, and instantly download it in seconds instead of building it from scratch.

---

## 🚀 How to Use This Cache

To consume the pre-built binaries on your local machine, add the following to your Nix configuration.

### Option A: Using `flake.nix` (NixOS / nix-darwin)

Add the substituter and public key directly to your flake's configuration block:

```nix
{
  nixConfig = {
    extra-substituters = [
      "https://sosumappu.cachix.org"
    ];
    extra-trusted-public-keys = [
      "sosumappu.cachix.org-1:xt9vvdkqXA3b2/DNj+VV77aWoveHXxlHMd3H3LmYL/c="
    ];
  };

  # ... rest of your flake
}
```
