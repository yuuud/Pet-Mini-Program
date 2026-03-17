# Pet-Mini-Program
# 🐾 Pet Avatar Mini Program – MVP Task Checklist (Route A)

> 技术路线：微信小程序 + Serverless（云开发 / 云函数）  
> 目标：最短时间跑通「登录 → 宠物档案 → 上传照片 → 卡通头像生成 → 查看结果」闭环

---

## ✅ Phase 0：MVP 范围确认（不写代码）

### 必须包含
- [ ] 微信授权登录（openid）
- [ ] 用户可创建 / 切换多只宠物
- [ ] 宠物基础信息（姓名、品种、生日、性别）
- [ ] 上传宠物照片
- [ ] 生成 **1 种风格** 的卡通头像
- [ ] 查看生成状态与历史结果

### 明确不做（本期）
- [ ] 社交 / 分享 / 点赞
- [ ] 多风格切换
- [ ] 支付 / 订阅
- [ ] 宠物医疗 / 行为分析

---

## ✅ Phase 1：数据结构设计（高优先级）

### 核心数据表

#### User
- [ ] openid
- [ ] unionid（预留）
- [ ] created_at

#### Pet
- [ ] pet_id
- [ ] user_id
- [ ] name
- [ ] type（dog / cat）
- [ ] breed
- [ ] birthday
- [ ] gender
- [ ] avatar_current_url

#### ImageAsset
- [ ] asset_id
- [ ] pet_id
- [ ] type（original / cartoon）
- [ ] url
- [ ] created_at

#### AvatarJob（异步任务）
- [ ] job_id
- [ ] pet_id
- [ ] source_image_id
- [ ] status（pending / running / success / failed）
- [ ] result_image_url
- [ ] error_msg
- [ ] created_at

---

## ✅ Phase 2：小程序前端（最小页面集）

### 页面
- [ ] 登录 / 授权页
- [ ] 宠物列表页（切换宠物）
- [ ] 宠物详情页（信息 + 当前头像）
- [ ] 照片上传页
- [ ] 生成结果页（生成中 / 成功 / 失败）

### 关键交互
- [ ] 上传后立即显示「生成中」
- [ ] 明确提示生成需要时间（避免同步等待）
- [ ] 支持刷新查看状态

---

## ✅ Phase 3：云函数 / API 能力

### Auth
- [ ] login()：openid 校验 + 自动注册

### Pet
- [ ] createPet()
- [ ] listPets()
- [ ] switchPet()

### Media
- [ ] getUploadToken()
- [ ] saveImageMeta()

### AvatarJob
- [ ] createJob()
- [ ] getJobStatus()

---

## ✅ Phase 4：图片存储策略

- [ ] 原图单独目录（限制大小 & 分辨率）
- [ ] 卡通结果图单独目录
- [ ] 文件名包含 pet_id / job_id
- [ ] 图片不直接存数据库，仅存 URL

---

## ✅ Phase 5：卡通头像生成（异步）

### 处理流程
1. [ ] 前端上传图片 → 获取 asset_id
2. [ ] 创建 AvatarJob（status = pending）
3. [ ] 异步触发 AI 生成（队列 / 延迟任务）
4. [ ] AI 返回结果 → 更新 Job 状态
5. [ ] 前端轮询 / 刷新查看结果

### AI 接入约束
- [ ] 每用户每日生成次数限制
- [ ] 失败兜底提示
- [ ] 仅支持 1 种风格（MVP）

---

## ✅ Phase 6：基础安全 & 风控

- [ ] 图片大小限制
- [ ] 上传频率限制
- [ ] 单用户并发 Job 数限制
- [ ] pet_id 必须属于当前 user
- [ ] 基础图片内容审核

---

## ✅ Phase 7：MVP 验收标准

- [ ] 新用户 ≤ 1 分钟完成首次流程
- [ ] 生成成功后可再次查看历史头像
- [ ] AI 失败有明确错误提示
- [ ] 上传失败支持重试

---

## ✅ 推荐推进顺序（现实版）

- Day 1：MVP 范围 + 数据模型
- Day 2–3：登录 / 宠物管理 / 图片上传
- Day 4–5：异步 Job + AI 生成
- Day 6–7：限流 / 异常处理 / UI 优化
