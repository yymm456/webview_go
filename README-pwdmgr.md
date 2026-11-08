# webview_go - pwdmgr 维护版

上游：https://github.com/webview/webview_go
基线：v0.0.0-20240831120633-6173450d4dd6（上游最新提交，2024-08-31）

pwdmgr 项目通过 go.mod 的 replace 引用本仓库：

    replace github.com/webview/webview_go => github.com/yymm456/webview_go v0.0.1

## 与上游的差异

1. libs/webview/include/webview.h 的 win32_edge_engine::embed() 中，
   在 put_IsStatusBarEnabled(FALSE) 之后调用 put_AreDefaultContextMenusEnabled(FALSE)，
   关闭 WebView2 原生右键菜单。该菜单由浏览器进程在宿主侧弹出，
   页面的 contextmenu 加 preventDefault() 无法阻止。
   搜索 [本地改动 pwdmgr] 可定位补丁。
2. go.mod 的 module 路径改为本仓库路径，webview.go 中 libs 的空导入同步修改。

其余文件与上游基线一致。

## 维护

改动代码后打新 tag，并更新 pwdmgr 项目 go.mod 中 replace 的版本号。
同步上游新版本时，覆盖代码后重做上述两处改动，再打新 tag。
