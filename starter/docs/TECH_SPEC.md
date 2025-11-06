# 技术规格 — Kids Story Studio

## 栈
- 前端：Next.js + Tailwind（或静态站）
- 文本：Handlebars 模板 + 语言模型润色（可选）
- 插画：固定风格（稳定扩散或可商用素材+滤镜）
- 排版：HTML → PDF（Puppeteer）
- 音频：TTS（Azure/ElevenLabs/Edge-tts）
- 支付与交付：Gumroad/LemonSqueezy Webhook → 邮件（Resend/Sendgrid）

## 流程
1) 表单收集参数
2) 生成故事文本（模板骨架 + 变量 + AI 润色）
3) 生成插画（每页提示词 → 固定风格图片）
4) HTML 排版 → PDF（封面/页码/扉页/封底）
5) 可选 TTS 音频（逐页合成 → 合并）
6) Webhook 触发生成/读取缓存 → 邮件下载链接

## 数据结构
{
  "orderId": "gm_123",
  "profile": {"name":"AnAn","age":6,"traits":["curious","brave"],"animal":"fox","city":"Chengdu","lang":"zh"},
  "theme": "adventure",
  "assets": {"pages": [{"text":"...","image":"s3://...","voice":"..."}],"pdf":"s3://...","zip":"s3://..."}
}

## 关键模块
- templates/：故事文本与插画提示词模板
- render/pdf：HTML 模板与样式
- webhook/handle：支付回调与任务队列
- storage：对象存储（S3/Cloudflare R2）

## 隐私与合规
- 不存储支付信息（由平台处理）
- 删除请求支持（GDPR 友好）

---