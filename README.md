# VPN Gate 家宽节点自动优选（edgetunnel 链式代理）

每 30 分钟由 GitHub Actions 抓取 [VPN Gate](https://www.vpngate.net/cn/) 的 SSTP 节点，
调用自己部署的 Cloudflare 检测 Worker 验证可用性与出口类型，生成可直接粘贴进
edgetunnel 后台「自定义优选IP」的清单。

产物：

```text
<本仓库 Pages 地址>/hosts.txt     粘贴进 edgetunnel 后台「自定义优选IP」
<本仓库 Pages 地址>/chains.txt    链式代理指令（粘贴进节点备注）
<本仓库 Pages 地址>/              按国家分组的可视化页面
```

## 本部署的改动（相对 hezhanleiok/gate）

- 检测 Worker 换成自建的 `https://gatecheck.cqcbfsq.workers.dev`（源码 [lsh8848/cm-Workers-CheckSocks5](https://github.com/lsh8848/cm-Workers-CheckSocks5)）。
- 配置全部放在 `.github/workflows/check.yml` 的环境变量里，`vpngate.py` 保持上游原样，方便以后同步。
- 发布前删除 `public/sub.txt`：`sub.txt` 里含明文 `vless://<UUID>@<域名>`，
  仓库和 Pages 都是公开的，等于把节点钥匙公开。改用 `hosts.txt` + 后台粘贴，
  UUID 不出现在任何公开文件里。
- `chains.txt` / `hosts.txt` 的固定地址由 `$GITHUB_REPOSITORY` 自动推导，fork 后无需手改。

## 手动触发

Actions → VPN Gate Node Check → Run workflow。
