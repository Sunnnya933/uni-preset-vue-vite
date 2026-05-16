# 可爱小猪登录页面

## 📱 页面说明

这是一个基于 **uniapp + Vue3** 的可爱小猪风格登录页面，适用于微信小程序。

## 🎨 设计特点

- **粉嫩配色**：采用粉红色系（#ffb3d9、#ff8cc6、#d4a5ff）
- **可爱形象**：定制 SVG 小猪吉祥物
- **流畅交互**：输入聚焦高亮、错误抖动、加载动画、成功反馈
- **响应式设计**：使用 rpx 单位，适配不同屏幕

## 📂 文件结构

```
src/
├── pages/
│   └── login/
│       └── index.vue          # 登录页面
├── static/
│   └── pig-mascot.svg         # 小猪 SVG 图标
└── pages.json                 # 页面配置（已更新）
```

## 🚀 使用方法

### 1. 启动项目

```bash
# 安装依赖（如果还没安装）
npm install

# 运行到微信小程序
npm run dev:mp-weixin
```

### 2. 测试账号

- **账号**：`piggy`
- **密码**：`123456`

### 3. 功能说明

- ✅ 账号/密码输入验证
- ✅ 密码显示/隐藏切换
- ✅ 输入错误抖动提示
- ✅ 登录加载状态
- ✅ 登录成功动画
- ✅ Toast 消息提示
- ✅ 忘记密码（占位）
- ✅ 立即注册（占位）

## 🎯 交互细节

1. **输入聚焦**：输入框获得焦点时，错误状态自动清除
2. **密码切换**：点击眼睛图标可切换密码显示/隐藏
3. **表单验证**：空值提交时，输入框抖动并显示 Toast
4. **登录流程**：
   - 点击登录按钮
   - 按钮显示"登录中..."（1.2秒）
   - 验证成功后显示成功遮罩（2.5秒）
   - 自动跳转到首页

## 🔧 自定义修改

### 修改配色

在 `login/index.vue` 的 `<style>` 中修改颜色变量：

```scss
// 主色调
background: linear-gradient(135deg, #ffe6f7 0%, #ffd4ed 40%, #ffe0f5 100%);

// 按钮颜色
background: linear-gradient(135deg, #ffb3d9 0%, #ff8cc6 100%);
```

### 对接真实 API

在 `handleLogin` 方法中替换模拟登录逻辑：

```javascript
const handleLogin = async () => {
  // ... 验证代码 ...
  
  isLoading.value = true
  
  try {
    // 调用真实登录 API
    const res = await uni.request({
      url: 'https://your-api.com/login',
      method: 'POST',
      data: {
        account: account.value,
        password: password.value
      }
    })
    
    if (res.data.success) {
      // 保存 token
      uni.setStorageSync('token', res.data.token)
      
      // 显示成功
      showSuccess.value = true
      setTimeout(() => {
        uni.switchTab({ url: '/pages/index/index' })
      }, 2500)
    } else {
      passwordError.value = true
      showToast(res.data.message || '登录失败')
    }
  } catch (error) {
    showToast('网络错误，请重试')
  } finally {
    isLoading.value = false
  }
}
```

## 📱 页面配置

登录页已设置为启动页（pages.json 第一项），如需修改：

```json
{
  "pages": [
    {
      "path": "pages/login/index",
      "style": {
        "navigationBarTitleText": "登录",
        "navigationStyle": "custom"  // 自定义导航栏
      }
    }
  ]
}
```

## 🎨 设计规范

- **画布宽度**：750rpx（标准小程序宽度）
- **安全区内边距**：40rpx
- **输入框高度**：约 96rpx（含 padding）
- **按钮高度**：约 84rpx
- **圆角**：24-28rpx
- **字体大小**：
  - 标题：40rpx
  - 输入：28rpx
  - 辅助文字：22rpx

## 📝 注意事项

1. **SVG 支持**：小程序支持 SVG，但建议转为 base64 或使用 image 组件
2. **动画性能**：使用 CSS 动画，性能良好
3. **输入框**：小程序的 input 组件样式有限制，已做适配
4. **导航栏**：使用 `navigationStyle: "custom"` 隐藏默认导航栏

## 🔗 相关链接

- [uniapp 官方文档](https://uniapp.dcloud.io/)
- [Vue3 文档](https://cn.vuejs.org/)
- [微信小程序文档](https://developers.weixin.qq.com/miniprogram/dev/framework/)

---

💖 享受可爱的小猪登录体验吧！
