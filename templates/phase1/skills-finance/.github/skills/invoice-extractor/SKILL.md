---
name: invoice-extractor
description: 从发票 PDF 或图片中提取结构化信息。当用户说"提取发票"、"报销扫描"、"发票识别"时触发。
version: "1.0"
author: "finance-platform"
license: "Apache-2.0"
---

# Invoice Extractor

从发票中提取商户名称、金额、日期、税率等结构化信息。

## 使用场景

- 用户上传发票图片并要求"提取信息"
- 用户说"报销发票"、"扫描票据"
- 用户需要将发票信息录入系统

## 不适用场景

- 手写发票（识别率低）
- 非中文发票
- 文件大于 10MB

## 使用方法

```python
from scripts.run import process
result = process("invoice.pdf")
```
