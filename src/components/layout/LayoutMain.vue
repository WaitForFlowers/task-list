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
                            <li class="nav-item">
                                首页
                            </li>
                            <li class="nav-item">
                                任务
                            </li>
                            <li class="nav-item">
                                报告
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
                <div class="nav-side-top">
                    <span class="nav-side-item-top nav-link d-flex align-items-center">
                        <i class="bi bi-house-door"></i><span class="menu-text">主菜单</span>
                    </span>
                </div>
                <nav class="nav-side-menu">
                    <ul class="nav nav-pills flex-column">
                        <li class="nav-item nav-side-item">
                            <a class="nav-link d-flex align-items-center active" data-bs-toggle="collapse" href="#menu1"
                                role="button">
                                <i class="bi bi-menu-button-wide"></i>
                                <span class="menu-text">侧边栏菜单项</span>
                            </a>
                            <div class="collapse" id="menu1">
                                <ul class="nav flex-column ms-1">
                                    <li class="nav-item">
                                        <a class="nav-link" href="#"><i class="bi bi-journal-text"></i><span
                                                class="menu-text">子菜单1</span></a>
                                    </li>
                                    <li class="nav-item">
                                        <a class="nav-link" href="#"><i class="bi bi-journal-text"></i><span
                                                class="menu-text">子菜单2</span></a>
                                    </li>
                                </ul>
                            </div>
                        </li>
                        <li class="nav-item nav-side-item">
                            <a class="nav-link d-flex align-items-center" data-bs-toggle="collapse" href="#menu1"
                                role="button">
                                <i class="bi bi-journal-text"></i><span class="menu-text">侧边栏菜单项2</span>
                            </a>

                        </li>
                        <li class="nav-item nav-side-item">
                            <a class="nav-link d-flex align-items-center" data-bs-toggle="collapse" href="#menu1"
                                role="button">
                                <i class="bi bi-journal-text"></i><span class="menu-text">侧边栏菜单项3</span>
                            </a>
                        </li>
                    </ul>
                </nav>
                <div class="nav-side-bottom show-hide" @click="toggleSidebar">
                    <i class="bi bi-chevron-double-left" v-if="!isCollapsed"></i>
                    <i class="bi bi-chevron-double-right" v-else></i>
                </div>
            </aside>
            <main class="main-content flex-grow-1 p-4 overflow-auto">
                <h1>欢迎来到任务列表系统</h1>
                <p>这是一个基于Vue3和Bootstrap5的任务管理系统。</p>
                <p>在这里，您可以创建、编辑和管理您的任务，轻松跟踪项目进度。</p>
                <p>使用侧边栏导航访问不同的功能模块，如任务列表、报告等。</p>
                <p>点击左侧的菜单图标可以展开或收起侧边栏，提升您的使用体验。</p>
            </main>

        </div>


    </div>
</template>
<script setup>
import { ref, computed, watch } from 'vue';
const isCollapsed = ref(false);
const toggleSidebar = () => {
    //点击隐藏侧边栏
    isCollapsed.value = !isCollapsed.value;
};
</script>
<style scoped>
.nav-top {
    height: 60px;
    background-color: #f8f9fa;
    display: flex;
    flex-direction: row;
    align-items: center;
    padding: 0 30px;
}

.nav-brand {
    width: 200px;
    height: 100%;
    background-color: blue;
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
    background-color: green;
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
    background-color: red;
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
    background-color: #f8f9fa;
    border-right: 1px solid rgba(0, 0, 0, 0.08);
    padding: 5px 5px;
    font-size: 1rem;
    font-weight: 600;
    color: #333;
    display: flex;
    flex-direction: column;
}

.nav-side-collapsed {
    width: 40px;
    /* 收起宽度，仅容纳图标 */
}

.nav-side-collapsed .menu-text {
    display: none;
}

.nav-side-collapsed .nav-link {
    padding: 5px 5px;
}

.nav-side-menu .nav .active {
    background: linear-gradient(145deg, #2563eb, #1d4ed8);
    color: white;
    box-shadow: 0 6px 12px rgba(37, 99, 235, 0.15);
}

.nav-side-item-top {
    padding: 5px 15px;
    border-radius: 5px;
}

.nav-item {
    border-radius: 5px;
    cursor: pointer;
}

.nav-item:hover {
    background-color: #e9ecef;
}

.collapse .nav-item:hover {
    background-color: #def084;
}

.nav-side-bottom {
    /* 固定在底部 */
    margin-top: auto;
    background-color: #1d4ed8;
    cursor: pointer;
}
</style>