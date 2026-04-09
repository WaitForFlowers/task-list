<template>
    <div class="d-flex flex-column vh-100 overflow-hidden">
        <!-- 顶部导航栏 (带折叠按钮) -->
        <nav class="navbar navbar-expand-lg navbar-dark bg-primary px-3 py-2 shadow-sm">
            <div class="container-fluid">
                <!-- 折叠切换按钮 (放在最左边) -->
                <button class="btn sidebar-toggle-btn text-white me-2 d-flex align-items-center" @click="toggleSidebar"
                    title="切换侧边栏">
                    <i class="bi" :class="sidebarVisible ? 'bi-layout-sidebar-inset' : 'bi-layout-sidebar'"></i>
                    <span class="d-none d-sm-inline ms-2 small fw-normal">{{ sidebarVisible ? '隐藏侧栏' : '显示侧栏' }}</span>
                </button>

                <a class="navbar-brand d-flex align-items-center" href="#">
                    <i class="bi bi-layers-fill me-2" style="font-size: 1.6rem;"></i>
                    <span class="fw-bold">Admin<span style="font-weight:300;">Hub</span></span>
                </a>

                <button class="navbar-toggler" type="button" data-bs-toggle="collapse"
                    data-bs-target="#navbarTopContent" aria-controls="navbarTopContent" aria-expanded="false"
                    aria-label="切换导航">
                    <span class="navbar-toggler-icon"></span>
                </button>

                <div class="collapse navbar-collapse" id="navbarTopContent">
                    <ul class="navbar-nav me-auto mb-2 mb-lg-0">
                        <li v-for="menu in topMenus" :key="menu.id" class="nav-item">
                            <a class="nav-link d-flex align-items-center" :class="{ active: activeTopMenu === menu.id }"
                                href="#" @click.prevent="selectTopMenu(menu.id)">
                                <i :class="'bi ' + menu.icon + ' me-2'"></i>
                                {{ menu.name }}
                            </a>
                        </li>
                    </ul>
                    <div class="d-flex align-items-center text-white-50">
                        <i class="bi bi-bell-fill me-3 fs-5"></i>
                        <div class="d-flex align-items-center">
                            <i class="bi bi-person-circle fs-4 me-2"></i>
                            <span class="d-none d-md-inline">管理员</span>
                        </div>
                    </div>
                </div>
            </div>
        </nav>

        <!-- 主体: 侧边栏 + 内容区 -->
        <div class="d-flex flex-grow-1 overflow-hidden">
            <!-- 左侧边栏 (v-show + 动态class实现折叠过渡) -->
            <aside class="sidebar d-flex flex-column py-3 overflow-auto" :class="{ collapsed: !sidebarVisible }"
                v-show="sidebarVisible">
                <div class="px-3 mb-3">
                    <span class="text-secondary text-uppercase small fw-semibold tracking-wide ps-2">
                        <i class="bi bi-menu-button-wide me-1"></i> {{ currentTopName }} 菜单
                    </span>
                </div>
                <nav class="nav nav-pills flex-column">
                    <a v-for="item in currentSidebarItems" :key="item.id" class="nav-link d-flex align-items-center"
                        :class="{ active: isSideActive(item) }" href="#" @click.prevent="selectSideItem(item)">
                        <i :class="'bi ' + item.icon"></i>
                        <span>{{ item.name }}</span>
                        <i v-if="isSideActive(item)" class="bi bi-chevron-right ms-auto opacity-75"
                            style="font-size:0.9rem;"></i>
                    </a>
                </nav>

                <div class="mt-auto px-3 pt-4">
                    <div class="alert alert-light border-0 bg-light-subtle rounded-4 small shadow-sm">
                        <i class="bi bi-info-circle-fill text-primary me-1"></i>
                        <strong>联动导航</strong><br>
                        <span class="text-secondary">点击顶部菜单，侧边栏自动切换；点击侧边栏，右侧内容更新。</span>
                    </div>
                </div>
            </aside>

            <!-- 右侧主要内容区 (自动占满剩余宽度) -->
            <main class="main-content flex-grow-1 p-4 overflow-auto">
                <div class="container-fluid px-0">
                    <!-- 页面标题区（如果侧边栏折叠，可以显示当前选中项提示） -->
                    <div class="d-flex align-items-center mb-4">
                        <h2 class="fw-semibold mb-0" style="color: #0b1e3a;">
                            <i class="bi" :class="selectedSideItem ? selectedSideItem.icon : 'bi-grid'"></i>
                            {{ selectedSideItem ? selectedSideItem.name : '欢迎' }}
                        </h2>
                        <span class="badge bg-primary bg-opacity-10 text-primary ms-3 px-3 py-2 rounded-pill fw-medium">
                            {{ currentTopName }}
                        </span>
                        <!-- 侧边栏折叠时显示小提示，点击按钮可展开 -->
                        <span v-if="!sidebarVisible" class="ms-3 text-secondary small">
                            <i class="bi bi-arrow-bar-left"></i> 侧边栏已隐藏，点击
                            <i class="bi bi-layout-sidebar"></i> 按钮可展开
                        </span>
                    </div>

                    <!-- 卡片区域: 展示联动状态 -->
                    <div class="row g-4">
                        <div class="col-12">
                            <div class="card preview-card p-3 p-md-4">
                                <div class="card-body">
                                    <div class="d-flex align-items-center mb-3">
                                        <div class="bg-primary bg-opacity-10 p-3 rounded-3 me-3">
                                            <i class="bi bi-arrow-left-right fs-3 text-primary"></i>
                                        </div>
                                        <div>
                                            <h5 class="card-title fw-bold mb-1">联动状态指示器</h5>
                                            <p class="card-text text-secondary mb-0">{{ displayMessage }}</p>
                                        </div>
                                    </div>
                                    <hr class="my-3">
                                    <div class="row">
                                        <div class="col-md-6">
                                            <div class="p-3 bg-light rounded-4">
                                                <span
                                                    class="text-secondary text-uppercase small fw-semibold">顶部导航</span>
                                                <div class="fs-5 fw-semibold mt-1">
                                                    <i class="bi"
                                                        :class="topMenus.find(m => m.id === activeTopMenu)?.icon || 'bi-speedometer2'"></i>
                                                    {{ currentTopName }}
                                                </div>
                                                <div class="text-secondary mt-2 small">
                                                    激活ID: <code>{{ activeTopMenu }}</code>
                                                </div>
                                            </div>
                                        </div>
                                        <div class="col-md-6 mt-3 mt-md-0">
                                            <div class="p-3 bg-light rounded-4">
                                                <span
                                                    class="text-secondary text-uppercase small fw-semibold">侧边栏选中项</span>
                                                <div class="fs-5 fw-semibold mt-1">
                                                    <i class="bi"
                                                        :class="selectedSideItem?.icon || 'bi-question-circle'"></i>
                                                    {{ selectedSideItem ? selectedSideItem.name : '未选中' }}
                                                </div>
                                                <div class="text-secondary mt-2 small">
                                                    标识: <code>{{ selectedSideItem?.id || 'none' }}</code>
                                                </div>
                                            </div>
                                        </div>
                                    </div>
                                    <div class="mt-4 p-3 border rounded-4 bg-white">
                                        <div class="d-flex">
                                            <i class="bi bi-chat-right-quote fs-2 me-3 text-primary opacity-50"></i>
                                            <div>
                                                <p class="mb-1 fw-medium">当前模块预览</p>
                                                <p class="text-secondary mb-0">
                                                    <span v-if="!selectedSideItem">👈 请点击左侧菜单查看详情</span>
                                                    <span v-else-if="activeTopMenu === 'dashboard'">
                                                        📊 正在展示「{{ selectedSideItem.name }}」的实时数据看板 ...
                                                    </span>
                                                    <span v-else-if="activeTopMenu === 'content'">
                                                        📝 内容管理 · {{ selectedSideItem.name }}：最近更新了12条记录。
                                                    </span>
                                                    <span v-else-if="activeTopMenu === 'settings'">
                                                        ⚙️ 系统设置 · {{ selectedSideItem.name }}：配置项已同步。
                                                    </span>
                                                    <span v-else>
                                                        ✨ 您正在浏览 {{ currentTopName }} 下的 {{ selectedSideItem.name }}。
                                                    </span>
                                                </p>
                                            </div>
                                        </div>
                                    </div>
                                    <div class="mt-3 text-end">
                                        <span class="badge bg-secondary bg-opacity-10 text-secondary py-2 px-3">
                                            <i class="bi bi-link-45deg"></i> 顶部「{{ currentTopName }}」⇢ 侧边「{{
                                                selectedSideItem?.name || '—' }}」
                                        </span>
                                    </div>
                                </div>
                            </div>
                        </div>

                        <div class="col-sm-6 col-lg-3" v-for="n in 4" :key="n">
                            <div class="card preview-card p-3">
                                <div class="d-flex align-items-center">
                                    <div class="flex-shrink-0 bg-primary bg-opacity-10 rounded-3 p-3">
                                        <i class="bi"
                                            :class="['bi-bar-chart', 'bi-people', 'bi-calendar-check', 'bi-clock-history'][n - 1]"></i>
                                    </div>
                                    <div class="flex-grow-1 ms-3">
                                        <h6 class="mb-0 fw-bold">{{ [128, 46, 89, 12][n - 1] }}</h6>
                                        <span class="text-secondary small">{{ ['访问量', '新用户', '任务', '进行中'][n - 1]
                                            }}</span>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </main>
        </div>
    </div>
</template>
<script setup>
import { ref, computed, watch } from 'vue';
// ---------- 顶部菜单配置 ----------
const topMenus = [
    { id: 'dashboard', name: '仪表盘', icon: 'bi-speedometer2' },
    { id: 'content', name: '内容管理', icon: 'bi-file-earmark-text' },
    { id: 'settings', name: '系统设置', icon: 'bi-gear-wide-connected' }
];

const activeTopMenu = ref('dashboard');

// ---------- 侧边栏菜单映射 ----------
const sidebarMenuMap = {
    dashboard: [
        { id: 'overview', name: '数据总览', icon: 'bi-grid-1x2-fill' },
        { id: 'analytics', name: '实时分析', icon: 'bi-bar-chart-line' },
        { id: 'monitor', name: '系统监控', icon: 'bi-activity' },
        { id: 'reports', name: '统计报表', icon: 'bi-file-spreadsheet' }
    ],
    content: [
        { id: 'posts', name: '文章管理', icon: 'bi-pencil-square' },
        { id: 'comments', name: '评论审核', icon: 'bi-chat-dots' },
        { id: 'media', name: '媒体库', icon: 'bi-camera-reels' },
        { id: 'categories', name: '分类标签', icon: 'bi-tags' }
    ],
    settings: [
        { id: 'profile', name: '个人资料', icon: 'bi-person-circle' },
        { id: 'security', name: '安全设置', icon: 'bi-shield-lock' },
        { id: 'notifications', name: '通知偏好', icon: 'bi-bell' },
        { id: 'team', name: '团队管理', icon: 'bi-people' }
    ]
};

const currentSidebarItems = computed(() => {
    return sidebarMenuMap[activeTopMenu.value] || sidebarMenuMap.dashboard;
});

const selectedSideItem = ref(null);

// 监听顶部切换，自动选中第一项
watch(activeTopMenu, (newTop) => {
    const items = sidebarMenuMap[newTop];
    if (items && items.length > 0) {
        selectedSideItem.value = items[0];
    } else {
        selectedSideItem.value = null;
    }
}, { immediate: true });

// ---------- 侧边栏折叠/展开状态 ----------
const sidebarVisible = ref(true);   // true=展开, false=折叠

const toggleSidebar = () => {
    sidebarVisible.value = !sidebarVisible.value;
};

// 方法
const selectTopMenu = (menuId) => {
    activeTopMenu.value = menuId;
};

const selectSideItem = (item) => {
    selectedSideItem.value = item;
};

const currentTopName = computed(() => {
    const menu = topMenus.find(m => m.id === activeTopMenu.value);
    return menu ? menu.name : '仪表盘';
});

const isSideActive = (item) => {
    return selectedSideItem.value && selectedSideItem.value.id === item.id;
};

const displayMessage = computed(() => {
    if (!selectedSideItem.value) return '请选择菜单项';
    return `当前导航: ${currentTopName.value} · ${selectedSideItem.value.name}`;
});


</script>
<style scoped>
body {
    font-family: 'Inter', system-ui, -apple-system, sans-serif;
    background-color: #f4f7fc;
    overflow: hidden;
}

/* 侧边栏基础样式 + 过渡动画 */
.sidebar {
    width: 280px;
    background-color: #ffffff;
    border-right: 1px solid rgba(0, 0, 0, 0.08);
    transition: width 0.25s ease-in-out, opacity 0.2s;
    box-shadow: inset -1px 0 0 rgba(0, 0, 0, 0.02);
    overflow: hidden;
    white-space: nowrap;
}

/* 折叠状态：宽度为0，但保留过渡 */
.sidebar.collapsed {
    width: 0 !important;
    border-right: none;
    opacity: 0;
    padding: 0 !important;
}

/* 内部内容在折叠时隐藏（避免溢出） */
.sidebar.collapsed * {
    display: none;
}

/* 侧边栏内部nav样式保持 */
.sidebar .nav-pills .nav-link {
    border-radius: 10px;
    margin: 4px 8px;
    padding: 10px 16px;
    color: #1e293b;
    font-weight: 500;
    transition: all 0.15s;
}

.sidebar .nav-pills .nav-link i {
    font-size: 1.2rem;
    margin-right: 12px;
    opacity: 0.8;
}

.sidebar .nav-pills .nav-link:hover {
    background-color: #f1f5f9;
    color: #0f172a;
}

.sidebar .nav-pills .nav-link.active {
    background: linear-gradient(145deg, #2563eb, #1d4ed8);
    color: white;
    box-shadow: 0 6px 12px rgba(37, 99, 235, 0.15);
}

.sidebar .nav-pills .nav-link.active i {
    opacity: 1;
    color: white;
}

/* 顶部导航微调 */
.navbar-brand {
    font-weight: 600;
    letter-spacing: -0.01em;
}

.navbar-nav .nav-link {
    font-weight: 500;
    padding: 0.6rem 1.2rem;
    border-radius: 30px;
    margin: 0 2px;
    transition: background 0.15s;
}

.navbar-nav .nav-link.active {
    background-color: rgba(255, 255, 255, 0.18);
    backdrop-filter: blur(4px);
}

.navbar-nav .nav-link i {
    margin-right: 8px;
}

/* 主内容区自适应 */
.main-content {
    background-color: #f8fafc;
    overflow-y: auto;
    transition: margin-left 0.25s ease-in-out;
}

/* 折叠按钮样式 */
.sidebar-toggle-btn {
    border-radius: 40px;
    padding: 0.3rem 0.8rem;
    transition: all 0.15s;
    border: 1px solid rgba(255, 255, 255, 0.3);
}

.sidebar-toggle-btn:hover {
    background-color: rgba(255, 255, 255, 0.15);
}

.sidebar-toggle-btn i {
    font-size: 1.3rem;
}

/* 卡片预览 */
.preview-card {
    border: none;
    border-radius: 20px;
    box-shadow: 0 8px 20px rgba(0, 0, 0, 0.02);
    background: white;
}

@media (max-width: 768px) {
    .sidebar {
        width: 240px;
    }
}
</style>