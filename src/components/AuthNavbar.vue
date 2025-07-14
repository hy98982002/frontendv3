<!-- AuthNavbar.vue - 用户登录后的导航栏组件 -->
<template>
  <nav
    class="navbar navbar-expand-lg navbar-light bg-white fixed-top"
    id="mainNav"
    :class="{ scrolled: isScrolled, hide: isHidden, show: !isHidden }"
  >
    <!-- 添加可调节的左右内边距容器 -->
    <div class="container-fluid custom-container">
      <a class="navbar-brand" href="javascript:void(0);">
        <img alt="UAI Logo" class="logo-img" :src="logoImg" />
      </a>

      <!-- 小屏幕时显示在右侧的图标组 -->
      <div class="d-flex d-lg-none ms-auto align-items-center">
        <!-- 购物车图标 -->
        <a href="javascript:void(0);" class="text-dark me-3" @click="openCart">
          <i class="iconfont icon-gouwuche nav-cart-icon"></i>
        </a>
        <!-- 通知图标 -->
        <a href="javascript:void(0);" class="text-dark position-relative mx-2">
          <i class="far fa-bell" style="font-size: 20px"></i>
          <span v-if="hasNotifications" class="badge-dot"></span>
        </a>
        <!-- 头像 -->
        <a href="javascript:void(0);" class="mx-2">
          <img :src="userAvatar" class="avatar-sm" alt="用户头像" />
        </a>
      </div>

      <!-- 折叠菜单按钮 -->
      <button
        class="navbar-toggler ms-2"
        type="button"
        data-bs-toggle="collapse"
        data-bs-target="#navbarSupportedContent"
        aria-controls="navbarSupportedContent"
        aria-expanded="false"
        aria-label="Toggle navigation"
      >
        <span class="navbar-toggler-icon"></span>
      </button>

      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul class="navbar-nav me-auto">
          <li class="nav-item active">
            <a class="nav-link" href="javascript:void(0);">
              <span class="nav-underline">首页</span>
              <span class="visually-hidden">(current)</span>
            </a>
          </li>
          <li class="nav-item dropdown">
            <a
              class="nav-link dropdown-toggle"
              href="javascript:void(0);"
              id="navbarDropdown1"
              role="button"
              data-bs-toggle="dropdown"
              aria-expanded="false"
            >
              <span class="nav-underline">职业变现探索</span>
            </a>
            <div class="dropdown-menu">
              <a class="dropdown-item" href="javascript:void(0);">AI+logo设计师</a>
              <a class="dropdown-item" href="javascript:void(0);">AI+包装师</a>
              <div class="dropdown-divider"></div>
              <a class="dropdown-item" href="javascript:void(0);">Something else here</a>
            </div>
          </li>
          <li class="nav-item dropdown">
            <a
              class="nav-link dropdown-toggle"
              href="javascript:void(0);"
              id="navbarDropdown2"
              role="button"
              data-bs-toggle="dropdown"
              aria-expanded="false"
            >
              <span class="nav-underline">学习路径</span>
            </a>
            <div class="dropdown-menu">
              <a class="dropdown-item" href="javascript:void(0);">AI+logo设计师</a>
              <a class="dropdown-item" href="javascript:void(0);">AI+包装师</a>
              <div class="dropdown-divider"></div>
              <a class="dropdown-item" href="javascript:void(0);">Something else here</a>
            </div>
          </li>
        </ul>
        <!-- 右侧整体区域 -->
        <div class="d-flex align-items-center right-group">
          <form class="d-flex my-2 my-lg-0 me-2 search-form-wrapper" @submit.prevent="search">
            <div class="position-relative" style="width: 450px; max-width: 100%">
              <input
                v-model="searchQuery"
                type="search"
                class="form-control search-input"
                placeholder="AI+logo设计师"
                aria-label="Search"
              />
              <button type="submit" class="search-btn-inside">
                <i class="fas fa-search"></i>
              </button>
            </div>
          </form>

          <!-- 购物车图标 -->
          <a href="javascript:void(0);" class="text-dark me-3 cart-link" @click="openCart">
            <i class="iconfont icon-gouwuche nav-cart-icon"></i>
            <span class="cart-underline"></span>
          </a>

          <!-- 通知图标 -->
          <a
            href="javascript:void(0);"
            class="text-dark position-relative mx-3"
            @click="handleNotificationClick"
          >
            <i class="far fa-bell" style="font-size: 20px"></i>
            <span v-if="hasNotifications" class="badge-dot"></span>
          </a>

          <!-- 用户头像下拉菜单 -->
          <div class="dropdown mx-3">
            <a
              href="javascript:void(0);"
              class="dropdown-toggle avatar-dropdown"
              data-bs-toggle="dropdown"
              aria-expanded="false"
              style="text-decoration: none"
            >
              <img :src="userAvatar" class="avatar-sm" alt="用户头像" />
            </a>
            <ul class="dropdown-menu user-dropdown">
              <li>
                <a @click="handleProfileClick" class="dropdown-item" href="javascript:void(0);"
                  >个人资料</a
                >
              </li>
              <li>
                <a @click="handleCoursesClick" class="dropdown-item" href="javascript:void(0);"
                  >我的课程</a
                >
              </li>
              <li>
                <a @click="handleOrdersClick" class="dropdown-item" href="javascript:void(0);"
                  >订单管理</a
                >
              </li>
              <li><hr class="dropdown-divider" /></li>
              <li>
                <a @click="handleLogout" class="dropdown-item" href="javascript:void(0);"
                  >退出登录</a
                >
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </nav>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'
import { useRouter } from 'vue-router'
import logoImg from '@/assets/icons/logo.png'
import defaultAvatar from '@/assets/images/avatars/tou03.png'

const router = useRouter()
const searchQuery = ref('')
const isScrolled = ref(false)
const isHidden = ref(false)
const hasNotifications = ref(true)
const userAvatar = ref(defaultAvatar)
let lastScrollY = 0

// 搜索方法
const search = () => {
  if (searchQuery.value.trim()) {
    console.log('搜索:', searchQuery.value)
  }
}

// 购物车方法
const openCart = () => {
  console.log('打开购物车')
  // router.push('/cart')
}

// 通知点击
const handleNotificationClick = () => {
  console.log('通知点击')
  // TODO: 显示通知列表
}

// 个人资料点击
const handleProfileClick = () => {
  console.log('个人资料点击')
  // router.push('/profile')
}

// 我的课程点击
const handleCoursesClick = () => {
  console.log('我的课程点击')
  // router.push('/my-courses')
}

// 订单管理点击
const handleOrdersClick = () => {
  console.log('订单管理点击')
  // router.push('/orders')
}

// 退出登录
const handleLogout = () => {
  console.log('退出登录')
  // TODO: 实现登出功能
}

// 处理滚动事件
const handleScroll = () => {
  const currentScrollY = window.scrollY

  // 导航栏阴影效果
  isScrolled.value = currentScrollY > 0

  // 导航栏隐藏/显示效果
  if (currentScrollY > 300 && currentScrollY > lastScrollY) {
    isHidden.value = true
  } else {
    isHidden.value = false
  }

  lastScrollY = currentScrollY
}

// 生命周期钩子
onMounted(() => {
  window.addEventListener('scroll', handleScroll)

  // 初始化Bootstrap下拉菜单
  const initDropdowns = () => {
    const dropdownElementList = document.querySelectorAll('.dropdown-toggle')
    if (window.bootstrap) {
      dropdownElementList.forEach(dropdownToggle => {
        // 移除click事件，改为hover触发
        const dropdown = new window.bootstrap.Dropdown(dropdownToggle, {
          // 禁用点击切换
          toggle: false
        })

        // 为dropdown父元素添加鼠标事件
        const dropdownLi = dropdownToggle.parentElement
        if (dropdownLi) {
          dropdownLi.addEventListener('mouseenter', () => {
            dropdown.show()
          })
          dropdownLi.addEventListener('mouseleave', () => {
            dropdown.hide()
          })
        }
      })
    }
  }

  // 确保Bootstrap已加载
  if (window.bootstrap) {
    initDropdowns()
  } else {
    // 如果Bootstrap尚未加载，等待一段时间后再尝试
    setTimeout(initDropdowns, 500)
  }
})

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll)
})
</script>

<style scoped>
/* 搜索框样式 */
.search-input {
  padding-right: 50px; /* 为内部按钮留出空间 */
  border-radius: 25px;
  border: 1px solid #1e7f98; /* 将默认边框颜色改为1e7f98 */
  font-size: 14px;
  height: 42px; /* 增加搜索框高度，原来大约是38px */
  line-height: 42px; /* 匹配高度 */
  padding-top: 0;
  padding-bottom: 0;
}

.search-input:focus {
  border-color: #1e7f98;
  box-shadow: 0 0 0 0.2rem rgba(194, 219, 254, 0.5); /* 将光晕效果颜色改为c2dbfe */
  outline: none;
}

/* 购物车图标样式 */
.nav-cart-icon {
  font-size: 22px;
  color: #000;
  margin-right: -2px;
}

/* 放大镜按钮样式 */
.search-btn-inside {
  background: none;
  border: none;
  outline: none;
  position: absolute;
  right: 15px;
  top: 50%;
  transform: translateY(-50%);
  width: 30px;
  height: 30px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: all 0.3s ease;
  border-radius: 50%;
  z-index: 10;
}

.search-btn-inside i {
  font-size: 16px;
  color: #f0690e;
  transition: all 0.3s ease;
}

.search-btn-inside:hover i,
.search-btn-inside:focus i {
  color: #f37000;
  font-size: 18px;
  font-weight: bold;
  -webkit-text-stroke: 0.5px #f37000;
  text-stroke: 0.5px #f37000;
}

.search-btn-inside:hover {
  background-color: transparent;
}

/* 导航栏样式 */
.navbar {
  transition: all 0.3s ease;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.custom-container {
  width: 100%;
  padding-right: var(--bs-gutter-x, 1.5rem);
  padding-left: var(--bs-gutter-x, 1.5rem);
  margin-right: auto;
  margin-left: auto;
  max-width: calc(100% - 100px) !important;
}

.navbar.scrolled {
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.15);
}

.navbar.hide {
  transform: translateY(-100%);
}

.navbar.show {
  transform: translateY(0);
}

/* Logo 品牌区域样式 */
.navbar-brand {
  margin-right: 0 !important;
  margin-left: 0 !important;
  padding: 0 !important;
  position: relative !important;
  display: flex !important;
  align-items: center !important;
}

.logo-img {
  height: 28px;
  width: auto;
  transition: transform 0.3s ease;
  margin-top: -10px;
  margin-left: 1px;
}

.navbar-brand:hover .logo-img {
  transform: scale(1.05);
}

/* 导航链接样式 */
.navbar-nav .nav-link {
  padding: 0.5rem 1rem;
  position: relative;
  transition: all 0.3s ease;
}

/* 移除下划线动画效果 */
.nav-underline {
  position: relative;
  display: inline-block;
}

.nav-underline::after {
  display: none;
}

.nav-link:hover .nav-underline::after,
.nav-item.active .nav-underline::after {
  width: 0;
}

/* 去掉dropdown-toggle的淡蓝色外边框 */
.dropdown-toggle:focus {
  box-shadow: none !important;
  outline: none !important;
  border-color: transparent !important;
}

.nav-link:focus {
  box-shadow: none !important;
  outline: none !important;
  border-color: transparent !important;
}

/* 下拉菜单三角形方向变化 */
.dropdown-toggle::after {
  display: inline-block;
  margin-left: 0.255em;
  vertical-align: 0.255em;
  content: '';
  border-top: 0.3em solid;
  border-right: 0.3em solid transparent;
  border-bottom: 0;
  border-left: 0.3em solid transparent;
  transition: transform 0.2s;
}

/* 用户头像下拉菜单不显示三角形 */
.avatar-dropdown.dropdown-toggle::after {
  display: none !important;
}

.nav-item:hover .dropdown-toggle::after {
  transform: rotate(180deg);
}

/* 移除hover效果 */
.nav-cart-icon:hover {
  transform: none;
  color: #000;
}

/* 底部带横线的购物车图标 */
.cart-wrapper {
  position: relative;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.cart-link {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-decoration: none;
}

.cart-line {
  display: block;
  width: 24px;
  height: 2px;
  background-color: #333;
  margin-top: 2px;
  border-radius: 1px;
  transition: all 0.3s ease;
}

.cart-link:hover .cart-line {
  background-color: #f0690e;
  width: 28px;
}

/* 搜索框位置调整 */
.search-form-wrapper {
  transform: translateX(-50px);
}

/* 移动端优化 */
@media (max-width: 992px) {
  .right-group {
    width: 100%;
    justify-content: space-between;
    margin-top: 1rem;
  }
}

/* 购物车图标和下划线样式 */
.cart-link {
  position: relative;
  display: inline-block;
}

.cart-underline {
  position: absolute;
  bottom: 5px;
  left: 0;
  width: 100%;
  height: 1.5px;
  background-color: #000;
  transform: translateX(1px);
}

/* 用户头像样式 */
.avatar-sm {
  width: 34px;
  height: 34px;
  object-fit: cover;
  border: 2px solid #fff;
  box-shadow: 0 0 0 1px #e5e5e5;
  border-radius: 50%;
}

/* 红点提醒 */
.badge-dot {
  position: absolute;
  top: -2px;
  right: -2px;
  width: 8px;
  height: 8px;
  background: #f0490e;
  border-radius: 50%;
}

/* 用户下拉菜单样式 */
.user-dropdown {
  border-radius: 10px;
  padding: 0.5rem 0;
  min-width: 160px;
  margin-top: 0.5rem;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.user-dropdown .dropdown-item {
  font-size: 14px;
  color: #5f6368;
  padding: 0.5rem 1rem;
}

.user-dropdown .dropdown-item:hover {
  background: rgba(30, 127, 152, 0.07);
  color: #000;
}

.user-dropdown .dropdown-divider {
  margin: 0.5rem 0;
}

/* 头像下拉菜单箭头去除 */
.dropdown-toggle.avatar-dropdown::after {
  display: none;
}
</style>
