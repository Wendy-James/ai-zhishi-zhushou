# AI 知识助手前端原型

这是一个文档问答场景的前端原型，页面包含资料上传、问题输入、聊天记录和回答展示区域。项目重点不是提供完整模型服务，而是把 Document QA / LLM application 的前端状态、接口边界和交互流程整理清楚。

## 功能范围

- 选择 PDF、PPTX、DOCX 文档并提交到后端上传接口。
- 聊天式问答区域，用户可以输入问题并查看助手回复。
- 预留参考资料展示位置，方便后续接入检索结果或引用片段。
- 采用纯 HTML、CSS 和 JavaScript 编写，便于直接阅读和二次修改。

## Interface Contract

当前页面假设后端提供两个接口。

### `POST /upload`

请求方式：

```text
Content-Type: multipart/form-data
field: file
```

预期响应：

```json
{
  "success": "uploaded"
}
```

失败响应示例：

```json
{
  "error": "file type is not supported"
}
```

### `POST /ask`

请求体：

```json
{
  "question": "请总结这份文档的核心内容"
}
```

预期响应：

```json
{
  "answer": "回答正文",
  "references": "可选：引用片段或资料来源"
}
```

## Frontend Flow

```text
选择文件
  -> FormData 上传到 /upload
  -> 展示上传状态

输入问题
  -> POST /ask
  -> 展示用户消息
  -> 展示助手回答和 references
```

## 本地预览

可以直接在浏览器打开 `index.html` 查看页面。

如果要让上传和问答真正生效，需要另行准备后端接口和模型/检索服务。当前仓库不包含后端、向量库、RAG 检索或真实模型调用。

## 已知限制

- 演示版用 `innerHTML` 渲染消息内容，接入真实后端前需要补充内容净化，避免 XSS 风险。
- 当前没有文件大小、页数、解析失败、上传进度等完整状态处理。
- 当前没有身份认证、速率限制、隐私策略或数据保留设置。
- 当前没有单元测试或 E2E 测试。
- `references` 只是预留展示位，尚未定义结构化引用格式。

## 下一步

- 将 `references` 改成结构化数组，例如 `[{ title, snippet, page, score }]`。
- 补充 loading、empty、error、retry 状态。
- 增加 mock 后端或 FastAPI 示例，方便本地端到端演示。
- 替换直接 `innerHTML` 渲染，增加内容净化。
- 补充 Playwright 或 Cypress E2E 测试。
