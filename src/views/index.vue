<template>
    <div class="page-container">
        <div class="sidebar-menu toggle-others fixed">
            <div class="sidebar-menu-inner">
                <header class="logo-env">
                    <!-- logo -->
                    <div class="logo">
                        <!-- <div style="">123ICP</div> -->
                        <!-- <p style="font-size: 14px;color: #eee;">Dfinity 生态导航</p> -->
                        <a href="#" class="logo-expanded">
                            <img src="../assets/images/icp123_dark.png" width="100" alt="" />
                        </a>
                        <a href="#" class="logo-collapsed">
                            <img src="../assets/images/icp123_v_dark.png" width="40" alt="" />
                        </a>
                        <p class="logo-desc" style="font-size: 14px;color: #eee;">Dfinity 生态导航</p>
                    </div>
                    <div class="mobile-menu-toggle visible-xs">
                        <a href="#" data-toggle="user-info-menu">
                            <i class="linecons-cog"></i>
                        </a>
                        <a href="#" data-toggle="mobile-menu">
                            <i class="fa-bars"></i>
                        </a>
                    </div>
                </header>
                <!-- 侧边栏 -->
                <ul id="main-menu" class="main-menu">
                    <li v-for="(menu, idx) in items" :key="idx + 'item'">
                        <a v-if="idx !== 0" :href="'#'+transName(menu)" class="smooth" @click="moveSlow" style="font-size: 16px; border: 0">
                            <i :class="menu.icon"></i>
                            <span class="title">{{transName(menu)}}</span>
                        </a>
                        <ul v-if="menu.children">
                            <li v-for="(submenu, idx) in menu.children" :key="idx">
                                <a :href="'#'+transName(submenu)" class="smooth" @click="moveSlow" style="font-size: 16px;">
                                    <span class="title">{{transName(submenu)}}</span>
                                    <span v-show="submenu.is_hot"
                                        class="label label-pink pull-right hidden-collapsed">Hot</span>
                                </a>
                            </li>
                        </ul>
                    </li>
                    <!-- 关于本站 -->
                    <!-- <li class="submit-tag">
                        <router-link to="/about">
                            <i class="linecons-heart"></i>
                            <span class="tooltip-blue">关于本站</span>
                            <span class="label label-Primary pull-right hidden-collapsed">♥︎</span>
                        </router-link>
                    </li> -->
                </ul>
            </div>
        </div>

        <div class="main-content">
            <nav class="navbar user-info-navbar" role="navigation">
                <ul class="user-info-menu left-links list-inline list-unstyled">
                    <li class="hidden-sm hidden-xs">
                        <a href="#" data-toggle="sidebar"><i class="fa-bars"></i></a>
                    </li>
                </ul>
                <ul class="user-info-menu right-links list-inline list-unstyled">
                    <!-- <li class="hidden-sm hidden-xs">
                        <a href="#" data-toggle="sidebar"><i class="fa-bars"></i></a>
                    </li> -->
                    <li class="dropdown hover-line language-switcher">
                        <a href="#" class="dropdown-toggle" data-toggle="dropdown">
                            <img :src="lang.flag" /> {{ lang.name }}
                        </a>
                        <ul class="dropdown-menu languages">
                            <li :class="{active: langItem.key === lang.key}" v-for="langItem in langList"
                                :key="langItem.key">
                                <a href="#" @click="lang = langItem">
                                    <img :src="langItem.flag" /> {{ langItem.name }}
                                </a>
                            </li>
                        </ul>
                    </li>
                </ul>
                <!-- <ul class="user-info-menu right-links list-inline list-unstyled">
                    <li class="hidden-sm hidden-xs">
                        <a href="https://github.com/" target="_blank">
                            <i class="fa-github"></i> GitHub
                        </a>
                    </li>
                </ul> -->
            </nav>

            <div v-for="(item, idx) in items" :key="idx">
                <div v-if="item.web">
                    <WebItem :item="item" :transName="transName" />
                </div>
                <div v-else v-for="(subItem, idx) in item.children" :key="idx">
                    <WebItem :item="subItem" :transName="transName" />
                </div>
            </div>
            <Footer />
        </div>
    </div>
</template>

<script>
import WebItem from '../components/WebItem.vue'
import Footer from '../components/Footer.vue'
import items from '../assets/data'

export default {
    name: 'Index',
    components: {
        WebItem,
        Footer
    },
    data() {
        return {
            items,
            lang: {},
            langList: [
                {
                    key: 'zh',
                    name: '简体中文',
                    flag: './assets/images/flags/flag-cn.png'
                },
                {
                    key: 'en',
                    name: 'English',
                    flag: './assets/images/flags/flag-us.png'
                }
            ]
        }
    },
    created() {
        this.lang = this.langList[0]
    },
    methods: {
        transName(webItem) {
            return this.lang.key === 'en' ? webItem.en_name : webItem.name
        },
        moveSlow() {}
    }
}
</script>

<style>
@media screen and (min-width: 768px) {
    .sidebar-menu.collapsed .logo-desc {
        display: none;
    }
}
</style>