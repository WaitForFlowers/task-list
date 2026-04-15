<template>
    <div class="layout-main">
        <!-- 顶部导航栏 -->
        <nav class="nav-top">
            <div class="nav-brand">
                <a class="navbar-brand" href="#">
                    <img src="@/assets/brand.jpg" alt="brand">
                    <span class="fw-bold">Task<span style="font-weight:300;">List</span></span>
                </a>
            </div>
            <div class="nav-menu">
                <div class="nav-menu-top">
                    <span class="">环境：dev 版本：2026-04-09，id：001 日期：2026-04-09</span>
                </div>
                <div class="nav-menu-bottom">
                    <div class="icon-left">
                        <i class="bi bi-chevron-left" style="-webkit-text-stroke: 2px;"></i>
                    </div>
                    <div class="nav-menu-list">
                        <ul class="nav">
                            <li class="nav-item" 
                            v-for="item in menus" :key="item.code"
                            :class="{ 'active': item.code === activeTop }"
                            @click="activeTop = item.code">
                                {{ item.name }}
                            </li>
                        </ul>
                    </div>
                    <div class="icon-right">
                        <i class="bi bi-chevron-right" style="-webkit-text-stroke: 2px;"></i>
                    </div>
                </div>

            </div>
            <div class="nav-user">
                <i class="bi bi-bell-fill"></i>
                <i class="bi bi-person-lock"></i>
                <i class="bi bi-x-lg"></i>
            </div>
        </nav>
        <!-- 主体: 侧边栏 + 内容区 -->
        <div class="d-flex flex-grow-1 overflow-hidden">
            <aside :class="['nav-side', { 'nav-side-collapsed': isCollapsed }]">
                <div class="nav nav-pills av-item nav-side-top">
                    <span class="nav-side-item-top nav-link d-flex align-items-center">
                        <i class="bi bi-house-door"></i><span class="menu-text">主菜单</span>
                    </span>
                </div>
                <nav class="nav-side-menu">
                    <ul class="nav nav-pills flex-column">
                        <li class="nav-item nav-side-item" 
                        v-for="item in sideMenus" :key="item.code">
                            <a class="nav-link d-flex align-items-center" data-bs-toggle="collapse" :href=" '#'+ item.code "
                                role="button">
                                <i :class="['bi',item.icon]"></i>
                                <router-link :to="item.path" v-if="item.children" 
                                :class="['menu-text',{ 'active': item.code === selectMenu }]"
                                @click="selectMenu = item.code"
                                >{{ item.name }}</router-link>
                                <span v-else class="menu-text">{{ item.name }}</span>
                            </a>
                            <div class="collapse" :id="item.code" v-if="item.children">
                                <ul class="nav flex-column ms-1">
                                    <li class="nav-item"
                                    v-for="childMenu in item.children" :key="childMenu.code">
                                        <a class="nav-link" href="#">
                                            <i :class="['bi',childMenu.icon]"></i>
                                            <router-link :to="childMenu.path" 
                                            :class="['menu-text',{ 'active': childMenu.code === selectMenu }]" 
                                            @click="selectMenu = childMenu.code">{{ childMenu.name }}</router-link>
                                        </a>
                                    </li>
                                    
                                </ul>
                            </div>
                        </li>
                    </ul>
                </nav>
                <div class="nav-side-bottom d-grid">
                    <button class="btn btn-primary btn-sm" @click="toggleSidebar" title="切换侧边栏">
                        <i class="bi" :class="isCollapsed ? 'bi-layout-sidebar-inset' : 'bi-layout-sidebar'"></i>
                        <span class="d-none d-sm-inline ms-2 small fw-normal">{{ isCollapsed ? '' : '隐藏侧栏' }}</span>
                    </button>
                </div>
            </aside>
            <main class="main-content flex-grow-1 overflow-auto">
                <div class="main-nav">
                    <i class="bi bi-arrow-bar-right"></i>
                    <span>当前功能路径：</span>
                    <nav aria-label="breadcrumb">
                        <ol class="breadcrumb">
                            <li class="breadcrumb-item"><a href="#">Home</a></li>
                            <li class="breadcrumb-item"><a href="#">Library</a></li>
                            <li class="breadcrumb-item active" aria-current="page">Data</li>
                        </ol>
                    </nav>
                </div>
                <div class="main-page">
                    <router-view />
                </div>
                
            </main>

        </div>


    </div>
</template>
<script setup>
import { ref, computed, onMounted } from 'vue';
import { loadDynamicRoutes } from '@/router/index.js';
const isCollapsed = ref(false);
const toggleSidebar = () => {
    //点击隐藏侧边栏
    isCollapsed.value = !isCollapsed.value;
};


// 获取菜单数据,并加载动态路由
const menus = ref([])
const activeTop = ref('') // 当前顶部菜单 code
const selectMenu = ref('') // 当前选中的菜单 code
const sideMenus = computed(() => {
  return menus.value.find(m => m.code === activeTop.value)?.children || []
})

onMounted(async () => {
  fetch('/mock/menus.json')
  .then(res => res.json())
  .then(data => {
    console.log(data)
    menus.value = data
    activeTop.value = menus.value[0].code || ''
    loadDynamicRoutes(menus.value)
  })
})
</script>
<style scoped>
.nav-top {
    height: 60px;
    background-color: #0d6efd;
    display: flex;
    flex-direction: row;
    align-items: center;
    padding: 0 30px;
}

.nav-brand {
    width: 200px;
    height: 100%;
    padding: 10px;
}

.nav-brand img {
    height: 100%;
    width: auto;
    margin-right: 10px;
}

.nav-menu {
    width: 100%;
    height: 100%;
    font-weight: 600;
    color: #fff;
}

.nav-menu .nav-menu-top {
    height: 40%;
    display: flex;
    flex-direction: row;
    justify-content: flex-end;
}

.nav-menu .nav-menu-bottom {
    height: 60%;
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 0 20px;
}

.nav-menu-list {
    display: flex;
    flex-direction: row;
    align-items: center;
}

.nav-menu-list .nav-item {
    margin: 0 15px;
    cursor: pointer;
}

.nav-menu-list .nav-item:hover {
    color: chocolate;
}

.nav-user {
    width: 200px;
    height: 100%;
    padding: 5px 5px;
}

.nav-user i {
    margin: 0 10px;
    font-size: 30px;
    cursor: pointer;
}

/* 侧边栏样式 */
.nav-side {
    width: 200px;
    height: calc(100vh - 60px);
    background-color: #ffffff;
    border-right: 1px solid rgba(0, 0, 0, 0.08);
    padding: 2px 2px;
    font-size: 1rem;
    font-weight: 600;
    color: #333;
    display: flex;
    flex-direction: column;
}

/* 收起宽度，仅容纳图标 */
.nav-side-collapsed {
    width: 45px;
}

.nav-side-collapsed .menu-text {
    display: none;
}

.nav-side-collapsed .nav-link {
    padding: 10px 10px;
}

.nav-side-menu .nav-link {
    border-radius: 10px;
    color: #1e293b;
    font-weight: 500;
    transition: all 0.15s;
}

.nav-side-menu .nav-link i {
    font-size: 1.2rem;
    opacity: 0.8;
}

.nav-side-bottom {
    /* 固定在底部 */
    margin-top: auto;
}

/* 主内容区自适应 */
.main-content {
    background-color: #f8fafc;
    overflow-y: auto;
    transition: margin-left 0.25s ease-in-out;
}

.main-content .main-nav {
    height: 20px;
    display: flex;
    padding: 1px 2px;
    font-size: 14px;
}

.main-content .main-page {
    margin: 5px 5px;
}
</style>