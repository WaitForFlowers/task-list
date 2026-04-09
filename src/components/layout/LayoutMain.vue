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
        <aside class="nav-side">
            <div class="nav-side-top">
                <span class="nav-side-item">
                    <i class="bi bi-house-door"></i>主菜单
                </span>
            </div>
            <nav class="nav-side-menu">
                <ul class="nav flex-column">
                    <li class="nav-item">
                        <i class="bi bi-menu-button-wide me-1"></i> 侧边栏菜单项1
                    </li>
                    <li class="nav-item">
                        <i class="bi bi-journal-text"></i>侧边栏菜单项2
                    </li>
                    <li class="nav-item">
                        <i class="bi bi-journal-text"></i>侧边栏菜单项3
                    </li>
                </ul>
            </nav>
            <div class="nav-side-bottom">
                <i class="bi bi-chevron-double-left"></i><i class="bi bi-chevron-double-left"></i>
            </div>
        </aside>
    </div>
</template>
<script setup>
import { ref, computed, watch } from 'vue';

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

.nav-side {
    width: 200px;
    height: calc(100vh - 60px);
    background-color: #f8f9fa;
    border-right: 1px solid rgba(0, 0, 0, 0.08);
    padding: 5px 10px;
    font-size: 1rem;
    font-weight: 600;
    color: #333;
    display: flex;
    flex-direction: column;
}

.nav-side-item {
    display: flex;
    align-items: center;
    padding: 5px 15px;
    border-radius: 5px;
}

.nav-side-item:hover {
    background-color: #e9ecef;
}

.nav-side-item i {
    margin-right: 10px;
}
</style>