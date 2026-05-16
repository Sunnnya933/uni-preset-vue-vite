# 可爱小猪注册页面

## 📱 页面说明

这是一个与登录页面风格一致的注册页面，包含完整的表单验证和交互动画。

## 🎨 设计特点

- **统一风格**：与登录页保持一致的粉色系配色
- **紫色按钮**：注册按钮使用紫色渐变，与登录页区分
- **实时验证**：输入时实时验证并显示提示信息
- **友好提示**：绿色✓表示正确，红色提示错误信息

## ✨ 功能特性

### 1. 表单验证

#### 账号验证
- 只允许字母和数字
- 长度限制：4-16位
- 实时验证并显示提示

#### 密码验证
- 长度限制：6-20位
- 实时验证强度
- 支持显示/隐藏切换

#### 确认密码
- 自动对比两次密码
- 不一致时显示错误提示
- 修改密码时自动重新验证

### 2. 交互动画

- ✅ 小猪跳动动画
- ✅ 输入框错误抖动
- ✅ 实时验证提示
- ✅ 注册加载状态
- ✅ 成功反馈动画
- ✅ Toast 消息提示

### 3. 页面跳转

- **注册成功**：自动返回登录页
- **已有账号**：点击"立即登录"返回登录页
- **登录页**：点击"立即注册"跳转到注册页

## 📂 文件结构

```
src/
├── pages/
│   ├── login/
│   │   └── index.vue          # 登录页面（已更新跳转）
│   └── register/
│       └── index.vue          # 注册页面（新增）
├── static/
│   └── pig-mascot.svg         # 小猪 SVG 图标
└── pages.json                 # 页面配置（已更新）
```

## 🎯 验证规则

### 账号规则
```javascript
// 4-16位字母或数字
/^[a-zA-Z0-9]{4,16}$/
```

**示例**：
- ✅ `piggy123`
- ✅ `user2024`
- ❌ `pig` (太短)
- ❌ `pig_123` (包含特殊字符)

### 密码规则
```javascript
// 6-20位任意字符
length >= 6 && length <= 20
```

**示例**：
- ✅ `123456`
- ✅ `myPassword123`
- ❌ `12345` (太短)

## 🚀 使用流程

### 用户操作流程

1. **进入注册页**
   - 从登录页点击"立即注册"
   - 或直接访问注册页

2. **填写信息**
   - 输入账号（实时验证格式）
   - 输入密码（实时验证长度）
   - 确认密码（实时对比一致性）

3. **提交注册**
   - 点击"立即注册"按钮
   - 显示"注册中..."加载状态
   - 注册成功显示成功动画
   - 自动返回登录页

4. **返回登录**
   - 点击"已有账号？立即登录"
   - 直接返回登录页

## 🎨 样式差异

### 与登录页的区别

| 项目 | 登录页 | 注册页 |
|------|--------|--------|
| 标题 | 可爱小猪登录 | 加入猪猪家族 |
| 副标题 | 欢迎来到粉红猪猪的世界～ | 创建你的专属账号～ |
| 按钮颜色 | 粉色渐变 (#ffb3d9 → #ff8cc6) | 紫色渐变 (#d4a5ff → #b388ff) |
| 输入框数量 | 2个 | 3个 |
| 背景装饰 | 2个光斑 | 3个光斑 |
| 底部链接 | 立即注册 | 立即登录 |

## 🔧 对接真实 API

在 `handleRegister` 方法中替换模拟注册逻辑：

```javascript
const handleRegister = async () => {
  // ... 验证代码 ...
  
  isLoading.value = true
  
  try {
    // 调用真实注册 API
    const res = await uni.request({
      url: 'https://your-api.com/register',
      method: 'POST',
      data: {
        account: account.value.trim(),
        password: password.value
      }
    })
    
    if (res.data.success) {
      // 显示成功
      showSuccess.value = true
      setTimeout(() => {
        // 返回登录页
        uni.navigateBack({
          delta: 1
        })
        // 或者携带账号信息跳转
        // uni.redirectTo({
        //   url: `/pages/login/index?account=${account.value}`
        // })
      }, 2500)
    } else {
      showToast(res.data.message || '注册失败')
    }
  } catch (error) {
    showToast('网络错误，请重试')
  } finally {
    isLoading.value = false
  }
}
```

## 💡 高级功能建议

### 1. 账号唯一性检查

```javascript
// 失焦时检查账号是否已存在
const checkAccountExists = async () => {
  if (!validateAccount()) return
  
  try {
    const res = await uni.request({
      url: 'https://your-api.com/check-account',
      data: { account: account.value }
    })
    
    if (res.data.exists) {
      accountTip.value = '该账号已被注册'
      accountError.value = true
    }
  } catch (error) {
    console.error('检查失败', error)
  }
}
```

### 2. 密码强度提示

```javascript
const getPasswordStrength = (pwd) => {
  if (pwd.length < 6) return { text: '密码太短', color: '#ef5350' }
  if (pwd.length < 8) return { text: '✓ 密码强度：弱', color: '#ff9800' }
  if (/^[0-9]+$/.test(pwd)) return { text: '✓ 密码强度：弱', color: '#ff9800' }
  if (/[a-zA-Z]/.test(pwd) && /[0-9]/.test(pwd)) {
    return { text: '✓ 密码强度：中', color: '#66bb6a' }
  }
  return { text: '✓ 密码强度：强', color: '#4caf50' }
}
```

### 3. 注册成功后自动登录

```javascript
// 注册成功后直接登录
if (res.data.success) {
  // 保存 token
  uni.setStorageSync('token', res.data.token)
  uni.setStorageSync('userInfo', res.data.userInfo)
  
  showSuccess.value = true
  setTimeout(() => {
    // 直接跳转到首页
    uni.switchTab({
      url: '/pages/index/index'
    })
  }, 2500)
}
```

## 📱 页面跳转说明

### 跳转方式

```javascript
// 1. navigateTo - 保留当前页，可返回
uni.navigateTo({
  url: '/pages/register/index'
})

// 2. navigateBack - 返回上一页
uni.navigateBack({
  delta: 1  // 返回的页面数
})

// 3. redirectTo - 关闭当前页，跳转新页
uni.redirectTo({
  url: '/pages/login/index'
})

// 4. switchTab - 跳转到 tabBar 页面
uni.switchTab({
  url: '/pages/index/index'
})
```

### 当前使用

- **登录 → 注册**：`navigateTo`（可返回）
- **注册 → 登录**：`navigateBack`（返回上一页）
- **登录成功 → 首页**：`switchTab`（如果首页是 tabBar）

## 🎨 自定义样式

### 修改注册按钮颜色

```scss
.btn-register {
  // 改为粉色
  background: linear-gradient(135deg, #ffb3d9 0%, #ff8cc6 100%);
  box-shadow: 0 12rpx 40rpx rgba(255, 140, 198, 0.4);
  
  // 或改为蓝色
  background: linear-gradient(135deg, #64b5f6 0%, #42a5f5 100%);
  box-shadow: 0 12rpx 40rpx rgba(66, 165, 245, 0.4);
}
```

### 修改验证提示颜色

```scss
.tip {
  color: #a5d6a7;  // 成功提示（绿色）
}

.tip.error {
  color: #ef5350;  // 错误提示（红色）
}
```

## 📝 注意事项

1. **表单验证**：前端验证只是第一道防线，后端也需要验证
2. **密码安全**：实际项目中应该加密传输（HTTPS + 加密算法）
3. **错误处理**：根据后端返回的错误码显示对应提示
4. **防重复提交**：已通过 `isLoading` 状态禁用按钮

## 🎯 测试建议

### 测试用例

1. **正常注册**
   - 账号：`testuser`
   - 密码：`123456`
   - 确认密码：`123456`
   - 预期：注册成功

2. **账号格式错误**
   - 账号：`ab` (太短)
   - 预期：显示"账号为4-16位字母或数字"

3. **密码不一致**
   - 密码：`123456`
   - 确认密码：`654321`
   - 预期：显示"两次密码不一致"

4. **空值提交**
   - 不填写任何内容
   - 预期：显示"请输入账号哦～"

---

💖 享受可爱的注册体验吧！
