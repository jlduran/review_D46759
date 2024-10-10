# FreeBSD OCI images

Inspired by [D46759](https://reviews.freebsd.org/D46759).

## Instructions

> [!NOTE]
> A [patched] version of poudriere is needed.

1. Create a poudriere jail.  It is worth noting that a pkg-based jail
   also needs patching, so let's use a standard jail with a kernel for
   now.

       poudriere jail -c -j freebsd15 -m pkgbase=base_latest -U https://pkg.freebsd.org -v 15

2. Build the container.

       poudriere image -t oci -j freebsd15 -n latest

3. Upload to you favorite registry.  It is recommended to create a
   [classic GitHub Personal Access Token] with `write:packages`,
   `delete:packages`, and `read:packages` scopes.

       podman tag localhost/freebsd15-minimal:latest ghcr.io/<github-username>/freebsd15-minimal:latest
       podman login --username <github-username> --password-stdin < echo "$GITHUB_TOKEN"
       podman push ghcr.io/<github-username>/freebsd15-minimal:latest

[patched]: https://github.com/jlduran/poudriere/tree/oci
[classic GitHub Personal Access Token]: https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#creating-a-personal-access-token-classic
