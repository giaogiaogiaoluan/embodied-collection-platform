# 具身采集平台

这是一个基于 GitHub Pages + Supabase 的设备和人员管理平台示例项目。前端字段名与 Supabase 后端字段名保持一致。

## 功能

- 登录界面：仅允许 Supabase Auth 后台已创建账号登录
- 主界面：设备、出勤、投产、有效产出汇总
- 设备登记：设备号只填数字，自动显示 Isuit 前缀；支持设备调配到北京、荣成、安顺、保定或回寄客户
- 每日设备情况：按日期、地区、设备号、设备状态、影响产能原因登记；支持表格与一键复制报表
- 每日设备产能和采集情况：按设备和人员登记小时数据；支持一键复制四地产能报表
- 每日采集主任务：支持北京/荣成/保定与安顺分组生成两个可复制报表
- 设备保修登记：支持问题描述、截图上传、工单生成与按日期查看
- 在线文档跳转：保存表格名称和在线表格链接，点击跳转

## Supabase 初始化

1. 创建 Supabase 项目。
2. 打开 Supabase Dashboard -> SQL Editor。
3. 复制 `supabase/schema.sql` 全部内容并执行。
4. 打开 Supabase Dashboard -> Authentication -> Users，手动创建允许登录的账号。
5. 不要在前端增加注册入口。

## 本地运行

```bash
npm install
cp .env.example .env
```

编辑 `.env`：

```bash
VITE_SUPABASE_URL=https://你的项目id.supabase.co
VITE_SUPABASE_ANON_KEY=你的_supabase_anon_public_key
```

启动：

```bash
npm run dev
```

## 部署到 GitHub Pages

1. 将代码上传到 GitHub 仓库。
2. 修改 `package.json`，增加 homepage：

```json
"homepage": "https://你的GitHub用户名.github.io/仓库名"
```

3. 执行：

```bash
npm run deploy
```

4. 到 GitHub 仓库 Settings -> Pages，确认来源为 gh-pages 分支。

## 字段统一说明

前端表单 name、Supabase 表字段、报表生成逻辑统一使用以下字段：

- device_registry：device_number, current_location
- device_daily_status：record_date, area, device_id, device_status, impact_reason, note
- daily_capacity：record_date, area, device_id, operator_name, required_hours, delay_hours, estimated_collection_hours, upload_hours, note
- daily_tasks：task_date, area, task_names
- repair_records：repair_date, area, device_id, issue_description, screenshot_path, screenshot_url
- online_docs：doc_name, doc_url

## 安全提醒

- 前端只能使用 Supabase anon public key。
- 不要把 service_role key 写入前端代码或 GitHub 仓库。
- 所有业务表已启用 RLS，仅允许 authenticated 用户访问。
- `repair-screenshots` 当前为 public bucket，方便复制截图链接。如果截图敏感，请改为 private bucket 并使用 signed URL。
