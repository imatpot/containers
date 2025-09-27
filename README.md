# 📦 Containers

Collection of unofficial OCI container images.

## How does this work?

Every directory in [`containers/`](./containers) contains a `Dockerfile` and an exemplary `compose.yaml`.
A submodule pointing to the containerized project is usually present at `src/`.
These can be used as reference to set up local environments.

Additionally, each container's image is pushed to [my container registry](https://github.com/imatpot?tab=packages) for easy usage, e.g. in [NixOS](https://wiki.nixos.org/wiki/Docker#Docker_Containers_as_systemd_Services) via [`virtualisation.oci-containers`](virtualisation.oci-containers).
