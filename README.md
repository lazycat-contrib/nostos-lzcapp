# nostos-lzcapp

[Nostos](https://github.com/Christian-Gennari/Nostos) 的 LazyCat LPK 打包 —— 自托管阅读空间（.NET 单容器）。

## 形态

- 单容器镜像：`ghcr.io/christian-gennari/nostos`，端口 8080，数据在 `/data`
- 镜像交付：mirror 模式（`ghcr.1ms.run/christian-gennari/nostos:{tag}`）
- 版本发现：上游只有 mutable 的 `latest` tag（CI 通过后才发布）与不可变 `selfhosted-sha-<commit>`，
  因此按 mutable tag 规则配置：`channel: custom` + `sort: created` + `tag_regex: ^latest$`
  + `bump: patch`（digest 变化才递增 patch 版本，未变则 no-op）
- 无部署密码体系（应用内自行注册账号），无需免密注入
- 数据持久化：`/lzcapp/var/data` -> `/data`（含 nostos.db、书库、keys/ 密钥环）

## 结构

- `package.yml` — LPK v2 包元数据（`community.lazycat.app.nostos`）
- `lzc-manifest.yml` — 单服务 + upstreams，保留上游 DataProtection 密钥环路径
- `lzc-build.yml` — 构建配置（无 buildscript）
- `.github/lazycat-action.yml` — 镜像版本源（mutable latest + digest 比对），mirror 交付，仅私有商店
- `.github/workflows/lazycat.yml` — 每日自动跟版 + 发布喵喵商店
