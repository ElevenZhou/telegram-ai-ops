# 第三方源码快照

本目录存放评估用的第三方开源项目源码，**不随本仓库提交**（已在根目录 `.gitignore` 中排除）。

- 每个项目都有各自的许可证，不应纳入本仓库版本控制。
- 总体积约 500M。
- 评估结论见 [../evaluation.md](../evaluation.md)。

## 获取方式

本机 `github.com` 的 git 协议当前不可达，无法 `git clone`。这些快照通过 GitHub API 下载默认分支的 tar 归档得到，**不含 `.git` 历史**。各项目对应的 commit SHA 记录在评估报告中。

如需重新获取（已登录 `gh`）：

```bash
fetch() { gh api "repos/$1/tarball/HEAD" > /tmp/x.tgz && mkdir -p "$2" && tar -xzf /tmp/x.tgz -C "$2" --strip-components=1; }
fetch chatwoot/chatwoot chatwoot
fetch Biryukovlex/tghost tghost
fetch 0mgABear/susmessagebot susmessagebot
# 其余项目同理，仓库名见 evaluation.md
```

网络恢复后也可直接 `git clone` 以获得完整历史。
