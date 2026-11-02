# dsh-purge 版本存档索引

本仓库是 [kingcharleslzy-ai/dsh-purge](https://github.com/kingcharleslzy-ai/dsh-purge) 的 fork，
用于**按版本分层保存源码与发行包**，方便随时回退到某个能用的版本。

## 三层结构

| 层 | 位置 | 内容 | 用途 |
|---|---|---|---|
| **源码层** | `master` 分支 | 与上游最新提交保持一致（当前 1.6.69） | 看代码、对照补丁锚点、向上游提 PR |
| **版本层** | Tags `v1.6.x`（全部 158 个） | 每个版本对应的源码快照 | 回到任意历史版本的完整源码 |
| **发行层** | Releases `v1.6.60` ~ `v1.6.69` | `.tgz` 安装包 + `SHA256SUMS.txt` | 直接安装 / 回退，不用重新编译 |

> `master` 保持与上游一致、不加本地文件，是为了让后续同步始终能 fast-forward（见文末）。

## 版本对照

| 版本 | 发布日期 | 已验证宿主 | 安装包 |
|---|---|---|---|
| v1.6.60 | 2026-10-25 | 0.2.1-rc.1 / rc.2 | [下载](https://github.com/zhhw2006-byte/dsh-purge-/releases/tag/v1.6.60) |
| v1.6.61 | 2026-10-30 | 0.2.1-rc.1 / rc.2 | [下载](https://github.com/zhhw2006-byte/dsh-purge-/releases/tag/v1.6.61) |
| v1.6.62 | 2026-10-30 | 0.2.1-rc.1 / rc.2 / rc.5 | [下载](https://github.com/zhhw2006-byte/dsh-purge-/releases/tag/v1.6.62) |
| v1.6.63 | 2026-10-30 | 0.2.1-rc.1 / rc.2 / rc.5 | [下载](https://github.com/zhhw2006-byte/dsh-purge-/releases/tag/v1.6.63) |
| v1.6.64 | 2026-10-30 | 0.2.1-rc.1 / rc.2 / rc.5 | [下载](https://github.com/zhhw2006-byte/dsh-purge-/releases/tag/v1.6.64) |
| v1.6.65 | 2026-10-31 | 0.2.1-rc.1 / rc.2 / rc.5 | [下载](https://github.com/zhhw2006-byte/dsh-purge-/releases/tag/v1.6.65) |
| v1.6.66 | 2026-10-31 | 0.2.1-rc.1 / rc.2 / rc.5 | [下载](https://github.com/zhhw2006-byte/dsh-purge-/releases/tag/v1.6.66) |
| v1.6.67 | 2026-10-31 | 0.2.1-rc.1 / rc.2 / rc.5 | [下载](https://github.com/zhhw2006-byte/dsh-purge-/releases/tag/v1.6.67) |
| v1.6.68 | 2026-11-01 | 0.2.1-rc.1 / rc.2 / rc.5 | [下载](https://github.com/zhhw2006-byte/dsh-purge-/releases/tag/v1.6.68) |
| v1.6.69（最新） | 2026-11-01 | 0.2.1-rc.1 / rc.2 / rc.5 | [下载](https://github.com/zhhw2006-byte/dsh-purge-/releases/tag/v1.6.69) |

**关于 rc.5**：从 1.6.62 起，`0.2.1-rc.5` 才被加入 `QUALIFIED_HOST_VERSIONS`（82 条补丁源码级零漂移）。
1.6.61 及更早版本在 rc.5 上属于「未验证」，点「应用」宿主补丁可能失败——这正是 1.6.47 在 rc.5 上把应用打挂的原因。

## 安装指定版本

**方式一：网页下载**

打开对应 Release → 下载 `dsh-purge-community-<版本>.tgz` → 在插件目录执行迁移：

```
purge-community migrate "<下载到的 tgz 绝对路径>"
```

**方式二：命令行**

```
gh release download v1.6.62 -R zhhw2006-byte/dsh-purge- --pattern "*.tgz" -D .
purge-community migrate "<tgz 绝对路径>"
```

安装前插件会自动做快照；装完需要手动重开应用，不会自动重启。

## 回退

```
purge-community backup                 # 先手动存一份当前状态（可选）
purge-community rollback <备份ID>      # 回到某个快照
```

快照位于 `~/.dsh/dsh-purge-community/backups/`。

## 同步上游

`master` 没有本地提交，所以每次只需要：

```
git fetch upstream
git push origin upstream/master:refs/heads/master
git push origin --tags
```

`archive-index` 分支只放这份索引，不参与上游同步。
