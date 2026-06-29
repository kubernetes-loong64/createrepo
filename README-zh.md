## createrepo_c

<p align="center"><a href="README.md">English</a> | <a href="README-zh.md">中文</a></p>

本项目为 [createrepo_c](https://github.com/rpm-software-management/createrepo_c) 构建多架构 Docker 镜像，createrepo_c 是用于生成 RPM 仓库元数据的 createrepo 工具的 C 语言实现。

镜像基于 **Anolis OS**，支持以下架构：

| 架构         | 平台            |
|------------|---------------|
| 🐉 loong64 | linux/loong64 |
| 💻 amd64   | linux/amd64   |
| 📱 arm64   | linux/arm64   |

主要目标是将 createrepo_c 带到 **LoongArch (loong64)** 平台，同时也提供 amd64 和 arm64 版本以便使用。

## 分支与版本

| 分支      | createrepo_c 版本 | 状态    |
|---------|-----------------|-------|
| `1.2.4` | 1.2.4           | 活跃    |
| `main`  | —               | 仓库根目录 |

分支名对应上游 [createrepo_c](https://github.com/rpm-software-management/createrepo_c) 的发行版本。标签格式为 `<createrepo_c-版本>+<构建编号>`（例如 `1.2.4+2`）。

## Docker 镜像

镜像发布在 Docker Hub 的 [`kubernetesloong64/createrepo`](https://hub.docker.com/r/kubernetesloong64/createrepo) 命名空间下。

### 多架构清单

多架构清单会自动选择适合您平台的架构：

| 镜像标签                                        | 平台                                    |
|---------------------------------------------|---------------------------------------|
| `kubernetesloong64/createrepo:1.2.4-anolis` | linux/amd64、linux/arm64、linux/loong64 |

### 架构特定镜像

| 镜像标签                                                | 平台            |
|-----------------------------------------------------|---------------|
| `kubernetesloong64/createrepo:1.2.4-anolis-amd64`   | linux/amd64   |
| `kubernetesloong64/createrepo:1.2.4-anolis-arm64`   | linux/arm64   |
| `kubernetesloong64/createrepo:1.2.4-anolis-loong64` | linux/loong64 |

### 使用方式

从 `.rpm` 包目录创建新的 RPM 仓库：

```shell
docker run --rm -v $(pwd)/rpms:/rpms -v $(pwd)/repo:/repo kubernetesloong64/createrepo:1.2.4-anolis createrepo_c /rpms -o /repo
```

更新已有的仓库元数据（添加新包、移除失效条目）：

```shell
docker run --rm -v $(pwd)/rpms:/rpms -v $(pwd)/repo:/repo kubernetesloong64/createrepo:1.2.4-anolis createrepo_c /rpms -o /repo --update
```

镜像中同时提供了 `createrepo_c` 和 `createrepo`（符号链接）。

## 验证发布

- 发布文件使用 GPG 签名。
- 从 [keys.openpgp.org](https://keys.openpgp.org) 下载公钥。
- 指纹：[FCF8724722CCBF9F51B1FBE376532BE7E3013105](https://keys.openpgp.org/debug?q=FCF8724722CCBF9F51B1FBE376532BE7E3013105)
- [手动下载](https://keys.openpgp.org/vks/v1/by-fingerprint/FCF8724722CCBF9F51B1FBE376532BE7E3013105)

```shell
gpg --keyserver keys.openpgp.org --recv-keys FCF8724722CCBF9F51B1FBE376532BE7E3013105
echo "FCF8724722CCBF9F51B1FBE376532BE7E3013105:6:" | gpg --import-ownertrust
```

或者，手动下载公钥文件后导入：

```shell
gpg --import /tmp/xxx
```

## 许可证

[Apache License 2.0](LICENSE)
