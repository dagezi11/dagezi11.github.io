# 为什么闭源
因为我不想接收pr+一些我自己的考虑

# AIChat（myaichat）

AIChat 是基于 Flutter 的 OpenAI 兼容聊天客户端。
支持 1v1/群聊、角色卡与世界书、脚本与模型管理，
数据全本地持久化（Isar）。

## 功能概览

- OpenAI 兼容 Chat Completions（`POST {base_url}/chat/completions`），SSE 流式输出
- 会话管理：私聊/群聊、搜索/删除、消息编辑/复制/撤回/重答
- 会话导入/导出：本地文件一键迁移
- 群聊点名：手动触发指定角色回复，支持开场白与切换
- 灵犀建议：基于上下文生成用户下一句回复
- 角色工坊：Tavern Card v1/v2(部分v3) PNG 导入/导出、角色书/世界书管理
- Lorebook 触发：关键词/正则、递归扫描、优先级与 token 预算
- 文本生成预设：SillyTavern preset JSON 导入/编辑，新会话默认
- 脚本系统：正则脚本发送前/接收后处理，可在会话内挂载
- 调试可视化：思考面板、请求/响应 JSON、上下文水位线与截断提示
- Android 保活：流式输出期间前台服务（可选悬浮层），减少后台被杀
- 本地持久化：设置、会话、消息、角色、世界观、脚本均保存到 Isar

## 技术栈

- Flutter + Material 3
- 状态管理：flutter_riverpod
- 网络：dio（SSE 流式）
- 本地存储：isar
- 配置：toml（`assets/config/app_config.toml`）
- 富文本：markdown + flutter_html（必要时 WebView）
- 文件选择：file_selector / file_picker（导入导出、选择模型文件等）
- 本地推理：fllama（llama.cpp / GGUF）
- 权限：permission_handler（Android）

## 数据导入/导出

- 会话：会话列表页右上角「导入会话」，长按会话「导出会话」
- 角色：角色工坊支持导入 Tavern Card v1/v2（PNG/JSON）
- 世界书：角色工坊支持导入世界书（JSON），导出角色卡时可选择是否附带世界书快照
- 文本生成预设：支持导入 SillyTavern preset JSON 并设为新会话默认

## 角色与世界观

- 角色卡 PNG 导入/导出：兼容 Tavern Card v1/v2（chara/ccv3）以及部分v3
- 世界书/角色书条目：关键词/正则触发，支持递归扫描与优先级/预算
- 宏替换：`{{char}}`、`{{user}}`、`<bot>`、`<user>` 等(非与Tavern Card完全对齐)

## 本地推理引擎（GGUF）

支持加载本地 GGUF 模型进行离线推理（基于 `fllama` / `llama.cpp`）。

### 支持的功能

- 加载本地 GGUF 模型文件
- CPU / GPU (OpenCL) 硬件加速
- 流式输出与停止生成
- 多种聊天模板：ChatML、Llama2、Llama3、Gemma、Mistral
- 自动检测设备能力并推荐配置

### 支持的模型格式

- GGUF 格式
- 建议模型大小：7B 参数以下（根据设备内存）

### 硬件加速

| 设备类型 | 说明 |
|---------|------|
| CPU | 默认选项，兼容所有设备 |
| GPU (OpenCL) | Adreno/Mali/PowerVR GPU 加速 |