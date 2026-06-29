## createrepo_c

<p align="center"><a href="README.md">English</a> | <a href="README-zh.md">中文</a></p>

This project builds multi-architecture Docker images for [createrepo_c](https://github.com/rpm-software-management/createrepo_c) — a C implementation of the createrepo tool used to generate RPM repository metadata.

The images are based on **Anolis OS** and support the following architectures:

| Architecture | Platform      |
|--------------|---------------|
| 🐉 loong64   | linux/loong64 |
| 💻 amd64     | linux/amd64   |
| 📱 arm64     | linux/arm64   |

The primary motivation is to bring createrepo_c to the **LoongArch (loong64)** platform, while also providing amd64 and arm64 variants for convenience.

## Branches and versions

| Branch  | createrepo_c version | Status          |
|---------|----------------------|-----------------|
| `1.2.4` | 1.2.4                | Active          |
| `main`  | —                    | Repository root |

The branch name corresponds to the upstream [createrepo_c](https://github.com/rpm-software-management/createrepo_c) release version. Tags follow the pattern `<createrepo_c-version>+<build-number>` (e.g., `1.2.4+2`).

## Docker images

Images are published to Docker Hub under the [`kubernetesloong64/createrepo`](https://hub.docker.com/r/kubernetesloong64/createrepo) namespace.

### Multi-arch manifest

The multi-arch manifest automatically selects the correct architecture for your platform:

| Image tag                                   | Platform                              |
|---------------------------------------------|---------------------------------------|
| `kubernetesloong64/createrepo:1.2.4-anolis` | linux/amd64、linux/arm64、linux/loong64 |

### Architecture-specific images

| Image tag                                           | Platform      |
|-----------------------------------------------------|---------------|
| `kubernetesloong64/createrepo:1.2.4-anolis-amd64`   | linux/amd64   |
| `kubernetesloong64/createrepo:1.2.4-anolis-arm64`   | linux/arm64   |
| `kubernetesloong64/createrepo:1.2.4-anolis-loong64` | linux/loong64 |

### Usage

Create a new RPM repository from a directory of `.rpm` packages:

```shell
docker run --rm -v $(pwd)/rpms:/rpms -v $(pwd)/repo:/repo kubernetesloong64/createrepo:1.2.4-anolis createrepo_c /rpms -o /repo
```

Update existing repository metadata (add new packages, remove stale entries):

```shell
docker run --rm -v $(pwd)/rpms:/rpms -v $(pwd)/repo:/repo kubernetesloong64/createrepo:1.2.4-anolis createrepo_c /rpms -o /repo --update
```

Both `createrepo_c` and `createrepo` (symlink) are available in the image.

## Verifying releases

- Releases are signed with GPG.
- Download the public key from [keys.openpgp.org](https://keys.openpgp.org).
- Fingerprint: [FCF8724722CCBF9F51B1FBE376532BE7E3013105](https://keys.openpgp.org/debug?q=FCF8724722CCBF9F51B1FBE376532BE7E3013105)
- [Manual download](https://keys.openpgp.org/vks/v1/by-fingerprint/FCF8724722CCBF9F51B1FBE376532BE7E3013105)

```shell
gpg --keyserver keys.openpgp.org --recv-keys FCF8724722CCBF9F51B1FBE376532BE7E3013105
echo "FCF8724722CCBF9F51B1FBE376532BE7E3013105:6:" | gpg --import-ownertrust
```

Or download the key file manually and import it:

```shell
gpg --import /tmp/xxx
```

## License

[Apache License 2.0](LICENSE)
