<template>
  <view class="login-container">
    <!-- 背景装饰 -->
    <view class="blob blob-1"></view>
    <view class="blob blob-2"></view>
    
    <!-- 头部区域 -->
    <view class="login-header">
      <!-- 可爱小猪 SVG -->
      <view class="mascot-wrapper">
        <image class="mascot" src="/static/pig-mascot.svg" mode="aspectFit"></image>
      </view>
      <text class="title">可爱小猪登录</text>
      <text class="subtitle">欢迎来到粉红猪猪的世界～</text>
    </view>
    
    <!-- 表单区域 -->
    <view class="login-form">
      <!-- 账号输入 -->
      <view class="field">
        <text class="label">账号</text>
        <view class="input-wrap" :class="{ error: accountError }">
          <text class="input-icon">👤</text>
          <input 
            class="input" 
            v-model="account" 
            placeholder="手机号 / 邮箱"
            @focus="accountError = false"
          />
        </view>
      </view>
      
      <!-- 密码输入 -->
      <view class="field">
        <text class="label">密码</text>
        <view class="input-wrap" :class="{ error: passwordError }">
          <text class="input-icon">🔒</text>
          <input 
            class="input" 
            v-model="password" 
            :password="!showPassword"
            placeholder="请输入密码"
            @focus="passwordError = false"
          />
          <text class="toggle-pwd" @tap="togglePassword">
            {{ showPassword ? '🙈' : '👁' }}
          </text>
        </view>
      </view>
      
      <!-- 忘记密码 -->
      <view class="forgot">
        <text class="forgot-link" @tap="handleForgot">忘记密码？</text>
      </view>
      
      <!-- 登录按钮 -->
      <button 
        class="btn-login" 
        :class="{ loading: isLoading }"
        @tap="handleLogin"
        :disabled="isLoading"
      >
        <text v-if="isLoading">登录中...</text>
        <text v-else>登 录</text>
      </button>
    </view>
    
    <!-- 底部注册提示 -->
    <view class="login-footer">
      <text class="register-hint">
        还没有账号？
        <text class="register-link" @tap="handleRegister">立即注册</text>
      </text>
    </view>
    
    <!-- Toast 提示 -->
    <view class="toast" :class="{ show: toastVisible }">
      {{ toastMessage }}
    </view>
    
    <!-- 成功遮罩 -->
    <view class="success-overlay" :class="{ show: showSuccess }">
      <view class="check">✓</view>
      <text class="success-title">登录成功！</text>
      <text class="success-text">欢迎来到粉红猪猪的世界</text>
    </view>
  </view>
</template>

<script setup>
import { ref } from 'vue'

// 表单数据
const account = ref('')
const password = ref('')
const showPassword = ref(false)
const isLoading = ref(false)

// 错误状态
const accountError = ref(false)
const passwordError = ref(false)

// Toast
const toastVisible = ref(false)
const toastMessage = ref('')

// 成功状态
const showSuccess = ref(false)

// 切换密码显示
const togglePassword = () => {
  showPassword.value = !showPassword.value
}

// 显示 Toast
const showToast = (message) => {
  toastMessage.value = message
  toastVisible.value = true
  setTimeout(() => {
    toastVisible.value = false
  }, 2200)
}

// 忘记密码
const handleForgot = () => {
  showToast('忘记密码流程（待开发）')
}

// 注册
const handleRegister = () => {
  uni.navigateTo({
    url: '/pages/register/index'
  })
}

// 登录
const handleLogin = async () => {
  // 验证
  if (!account.value.trim()) {
    accountError.value = true
    showToast('请填写账号哦～')
    return
  }
  
  if (!password.value.trim()) {
    passwordError.value = true
    showToast('请填写密码哦～')
    return
  }
  
  // 开始登录
  isLoading.value = true
  
  // 模拟登录请求
  setTimeout(() => {
    isLoading.value = false
    
    // 演示：账号 piggy，密码 123456
    if (account.value.trim() === 'piggy' && password.value === '123456') {
      showSuccess.value = true
      setTimeout(() => {
        showSuccess.value = false
        // 跳转到首页
        uni.switchTab({
          url: '/pages/index/index'
        })
      }, 2500)
    } else {
      passwordError.value = true
      showToast('账号或密码错误，试试 piggy / 123456')
    }
  }, 1200)
}
</script>

<style lang="scss" scoped>
.login-container {
  min-height: 100vh;
  background: linear-gradient(135deg, #ffe6f7 0%, #ffd4ed 40%, #ffe0f5 100%);
  padding: 0 40rpx;
  position: relative;
  overflow: hidden;
}

/* 背景装饰 */
.blob {
  position: absolute;
  border-radius: 50%;
  opacity: 0.35;
  filter: blur(2rpx);
}

.blob-1 {
  width: 240rpx;
  height: 240rpx;
  background: #ffe6f2;
  top: -40rpx;
  left: -60rpx;
}

.blob-2 {
  width: 160rpx;
  height: 160rpx;
  background: #d4a5ff;
  top: 20rpx;
  right: -20rpx;
}

/* 头部 */
.login-header {
  padding: 72rpx 0 24rpx;
  text-align: center;
  position: relative;
  z-index: 2;
}

.mascot-wrapper {
  width: 176rpx;
  height: 176rpx;
  margin: 0 auto 20rpx;
  animation: bounce 2.5s ease-in-out infinite;
}

@keyframes bounce {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-8rpx); }
}

.mascot {
  width: 100%;
  height: 100%;
}

.title {
  display: block;
  font-size: 40rpx;
  font-weight: 900;
  color: #ff8cc6;
  letter-spacing: 0.02em;
  margin-bottom: 8rpx;
}

.subtitle {
  display: block;
  font-size: 22rpx;
  color: #9d8ca0;
}

/* 表单 */
.login-form {
  margin-top: 16rpx;
  position: relative;
  z-index: 2;
}

.field {
  margin-bottom: 28rpx;
}

.label {
  display: block;
  font-size: 22rpx;
  font-weight: 700;
  color: #5d4e60;
  margin-bottom: 12rpx;
}

.input-wrap {
  display: flex;
  align-items: center;
  background: #fff;
  border: 4rpx solid transparent;
  border-radius: 24rpx;
  box-shadow: 0 4rpx 24rpx rgba(0, 0, 0, 0.06);
  transition: all 0.2s;
}

.input-wrap.error {
  border-color: #ef5350;
  animation: shake 0.4s ease;
}

@keyframes shake {
  0%, 100% { transform: translateX(0); }
  25% { transform: translateX(-8rpx); }
  75% { transform: translateX(8rpx); }
}

.input-icon {
  width: 80rpx;
  text-align: center;
  font-size: 32rpx;
  flex-shrink: 0;
}

.input {
  flex: 1;
  padding: 24rpx 16rpx 24rpx 0;
  font-size: 28rpx;
  color: #5d4e60;
}

.toggle-pwd {
  width: 80rpx;
  height: 80rpx;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 32rpx;
  opacity: 0.6;
}

.forgot {
  text-align: right;
  margin-bottom: 28rpx;
}

.forgot-link {
  font-size: 22rpx;
  color: #d4a5ff;
  font-weight: 500;
}

.btn-login {
  width: 100%;
  padding: 28rpx;
  border: none;
  border-radius: 28rpx;
  font-size: 30rpx;
  font-weight: 800;
  color: #fff;
  background: linear-gradient(135deg, #ffb3d9 0%, #ff8cc6 100%);
  box-shadow: 0 12rpx 40rpx rgba(255, 140, 198, 0.4);
  letter-spacing: 0.08em;
  transition: all 0.2s;
}

.btn-login.loading {
  opacity: 0.85;
}

.btn-login::after {
  border: none;
}

/* 底部 */
.login-footer {
  text-align: center;
  margin-top: 48rpx;
  position: relative;
  z-index: 2;
}

.register-hint {
  font-size: 22rpx;
  color: #9d8ca0;
}

.register-link {
  color: #ffb3d9;
  font-weight: 700;
}

/* Toast */
.toast {
  position: fixed;
  bottom: 160rpx;
  left: 50%;
  transform: translateX(-50%) translateY(40rpx);
  background: rgba(45, 42, 38, 0.92);
  color: #fff;
  padding: 20rpx 36rpx;
  border-radius: 40rpx;
  font-size: 24rpx;
  opacity: 0;
  pointer-events: none;
  transition: all 0.3s;
  white-space: nowrap;
  z-index: 100;
}

.toast.show {
  opacity: 1;
  transform: translateX(-50%) translateY(0);
}

/* 成功遮罩 */
.success-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(255, 245, 251, 0.95);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  opacity: 0;
  pointer-events: none;
  transition: opacity 0.35s;
  z-index: 200;
}

.success-overlay.show {
  opacity: 1;
  pointer-events: auto;
}

.check {
  width: 128rpx;
  height: 128rpx;
  background: #a5d6a7;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 64rpx;
  color: #fff;
  margin-bottom: 24rpx;
  animation: pop 0.4s cubic-bezier(0.34, 1.56, 0.64, 1);
}

@keyframes pop {
  from { transform: scale(0); }
  to { transform: scale(1); }
}

.success-title {
  font-size: 36rpx;
  color: #ff8cc6;
  font-weight: 700;
  margin-bottom: 8rpx;
}

.success-text {
  font-size: 24rpx;
  color: #9d8ca0;
}
</style>
