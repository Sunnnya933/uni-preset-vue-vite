<template>
  <view class="register-container">
    <!-- 背景装饰 -->
    <view class="blob blob-1"></view>
    <view class="blob blob-2"></view>
    <view class="blob blob-3"></view>
    
    <!-- 头部区域 -->
    <view class="register-header">
      <!-- 可爱小猪 SVG -->
      <view class="mascot-wrapper">
        <image class="mascot" src="/static/pig-mascot.svg" mode="aspectFit"></image>
      </view>
      <text class="title">加入猪猪家族</text>
      <text class="subtitle">创建你的专属账号～</text>
    </view>
    
    <!-- 表单区域 -->
    <view class="register-form">
      <!-- 账号输入 -->
      <view class="field">
        <text class="label">账号</text>
        <view class="input-wrap" :class="{ error: accountError }">
          <text class="input-icon">👤</text>
          <input 
            class="input" 
            v-model="account" 
            placeholder="请输入账号（字母/数字）"
            @focus="accountError = false"
            @input="validateAccount"
          />
        </view>
        <text v-if="accountTip" class="tip" :class="{ error: accountError }">
          {{ accountTip }}
        </text>
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
            placeholder="请输入密码（6-20位）"
            @focus="passwordError = false"
            @input="validatePassword"
          />
          <text class="toggle-pwd" @tap="togglePassword">
            {{ showPassword ? '🙈' : '👁' }}
          </text>
        </view>
        <text v-if="passwordTip" class="tip" :class="{ error: passwordError }">
          {{ passwordTip }}
        </text>
      </view>
      
      <!-- 确认密码输入 -->
      <view class="field">
        <text class="label">确认密码</text>
        <view class="input-wrap" :class="{ error: confirmError }">
          <text class="input-icon">🔐</text>
          <input 
            class="input" 
            v-model="confirmPassword" 
            :password="!showConfirmPassword"
            placeholder="请再次输入密码"
            @focus="confirmError = false"
            @input="validateConfirm"
          />
          <text class="toggle-pwd" @tap="toggleConfirmPassword">
            {{ showConfirmPassword ? '🙈' : '👁' }}
          </text>
        </view>
        <text v-if="confirmTip" class="tip error">
          {{ confirmTip }}
        </text>
      </view>
      
      <!-- 注册按钮 -->
      <button 
        class="btn-register" 
        :class="{ loading: isLoading }"
        @tap="handleRegister"
        :disabled="isLoading"
      >
        <text v-if="isLoading">注册中...</text>
        <text v-else>立即注册</text>
      </button>
    </view>
    
    <!-- 底部登录提示 -->
    <view class="register-footer">
      <text class="login-hint">
        已有账号？
        <text class="login-link" @tap="goToLogin">立即登录</text>
      </text>
    </view>
    
    <!-- Toast 提示 -->
    <view class="toast" :class="{ show: toastVisible }">
      {{ toastMessage }}
    </view>
    
    <!-- 成功遮罩 -->
    <view class="success-overlay" :class="{ show: showSuccess }">
      <view class="check">✓</view>
      <text class="success-title">注册成功！</text>
      <text class="success-text">欢迎加入粉红猪猪家族</text>
    </view>
  </view>
</template>

<script setup>
import { ref, computed } from 'vue'

// 表单数据
const account = ref('')
const password = ref('')
const confirmPassword = ref('')
const showPassword = ref(false)
const showConfirmPassword = ref(false)
const isLoading = ref(false)

// 错误状态
const accountError = ref(false)
const passwordError = ref(false)
const confirmError = ref(false)

// 提示信息
const accountTip = ref('')
const passwordTip = ref('')
const confirmTip = ref('')

// Toast
const toastVisible = ref(false)
const toastMessage = ref('')

// 成功状态
const showSuccess = ref(false)

// 验证账号
const validateAccount = () => {
  const value = account.value.trim()
  if (!value) {
    accountTip.value = ''
    return false
  }
  
  // 只允许字母和数字，4-16位
  const regex = /^[a-zA-Z0-9]{4,16}$/
  if (!regex.test(value)) {
    accountTip.value = '账号为4-16位字母或数字'
    accountError.value = true
    return false
  }
  
  accountTip.value = '✓ 账号格式正确'
  accountError.value = false
  return true
}

// 验证密码
const validatePassword = () => {
  const value = password.value
  if (!value) {
    passwordTip.value = ''
    return false
  }
  
  if (value.length < 6 || value.length > 20) {
    passwordTip.value = '密码长度为6-20位'
    passwordError.value = true
    return false
  }
  
  passwordTip.value = '✓ 密码强度合格'
  passwordError.value = false
  
  // 如果已经输入了确认密码，重新验证
  if (confirmPassword.value) {
    validateConfirm()
  }
  
  return true
}

// 验证确认密码
const validateConfirm = () => {
  const value = confirmPassword.value
  if (!value) {
    confirmTip.value = ''
    return false
  }
  
  if (value !== password.value) {
    confirmTip.value = '两次密码不一致'
    confirmError.value = true
    return false
  }
  
  confirmTip.value = ''
  confirmError.value = false
  return true
}

// 切换密码显示
const togglePassword = () => {
  showPassword.value = !showPassword.value
}

const toggleConfirmPassword = () => {
  showConfirmPassword.value = !showConfirmPassword.value
}

// 显示 Toast
const showToast = (message) => {
  toastMessage.value = message
  toastVisible.value = true
  setTimeout(() => {
    toastVisible.value = false
  }, 2200)
}

// 跳转到登录页
const goToLogin = () => {
  uni.navigateBack({
    delta: 1
  })
}

// 注册
const handleRegister = async () => {
  // 验证账号
  if (!account.value.trim()) {
    accountError.value = true
    showToast('请输入账号哦～')
    return
  }
  
  if (!validateAccount()) {
    showToast('账号格式不正确')
    return
  }
  
  // 验证密码
  if (!password.value) {
    passwordError.value = true
    showToast('请输入密码哦～')
    return
  }
  
  if (!validatePassword()) {
    showToast('密码格式不正确')
    return
  }
  
  // 验证确认密码
  if (!confirmPassword.value) {
    confirmError.value = true
    showToast('请确认密码哦～')
    return
  }
  
  if (!validateConfirm()) {
    showToast('两次密码不一致')
    return
  }
  
  // 开始注册
  isLoading.value = true
  
  // 模拟注册请求
  setTimeout(() => {
    isLoading.value = false
    
    // 演示：注册成功
    showSuccess.value = true
    setTimeout(() => {
      showSuccess.value = false
      // 跳转到登录页
      uni.navigateBack({
        delta: 1
      })
      // 或者直接跳转到登录页
      // uni.redirectTo({
      //   url: '/pages/login/index'
      // })
    }, 2500)
  }, 1200)
}
</script>

<style lang="scss" scoped>
.register-container {
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

.blob-3 {
  width: 200rpx;
  height: 200rpx;
  background: #b3e5fc;
  bottom: 100rpx;
  left: 20rpx;
  opacity: 0.25;
}

/* 头部 */
.register-header {
  padding: 60rpx 0 20rpx;
  text-align: center;
  position: relative;
  z-index: 2;
}

.mascot-wrapper {
  width: 160rpx;
  height: 160rpx;
  margin: 0 auto 16rpx;
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
.register-form {
  margin-top: 16rpx;
  position: relative;
  z-index: 2;
}

.field {
  margin-bottom: 24rpx;
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

.tip {
  display: block;
  font-size: 20rpx;
  color: #a5d6a7;
  margin-top: 8rpx;
  margin-left: 12rpx;
}

.tip.error {
  color: #ef5350;
}

.btn-register {
  width: 100%;
  padding: 28rpx;
  border: none;
  border-radius: 28rpx;
  font-size: 30rpx;
  font-weight: 800;
  color: #fff;
  background: linear-gradient(135deg, #d4a5ff 0%, #b388ff 100%);
  box-shadow: 0 12rpx 40rpx rgba(179, 136, 255, 0.4);
  letter-spacing: 0.08em;
  transition: all 0.2s;
  margin-top: 16rpx;
}

.btn-register.loading {
  opacity: 0.85;
}

.btn-register::after {
  border: none;
}

/* 底部 */
.register-footer {
  text-align: center;
  margin-top: 40rpx;
  position: relative;
  z-index: 2;
}

.login-hint {
  font-size: 22rpx;
  color: #9d8ca0;
}

.login-link {
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
