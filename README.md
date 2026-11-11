# cue-workbuddy-skills

Cue WorkBuddy 技能的公开更新清单。技能运行时会读取 `versions.json` 比对版本号，发现新版本后询问用户是否更新。

## 目录结构

```
versions.json                      # 最新版本清单（技能读取）
releases/<slug>-<version>.zip      # 各版本技能包
```

`versions.json` 由本地发布库自动生成，不要手改。

## 技能如何发现更新

技能包内的 `scripts/check_skill_version.py` 会：

1. 读取包内 `version.json` 得到当前版本；
2. 拉取 `https://raw.githubusercontent.com/57bchck6mg-pixel/cue-workbuddy-skills/main/versions.json`（失败时回退 cdn.jsdelivr.net）；
3. 本地缓存 24 小时，失败静默放行；
4. 若清单里的版本更高，交付完任务后由技能弹出更新询问。

更新时下载 zip 并校验 sha256。WorkBuddy 技能市场管理的安装不会被原地覆盖，会引导用户在「技能中心 → 导入本地技能」导入；自管理目录会先备份再替换。

## 发布新版本

1. 在 WorkBuddy 打包配置里递增技能版本号；
2. 重新打包并同步到发布库；
3. 在 `workbuddy-upload` 目录执行：

```bash
python3 make_github_release.py --push
```
