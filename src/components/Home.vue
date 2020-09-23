<template>
    <el-container class="home-container">
        <!-- //头部 -->
        <el-header>
            <div>
                <img src="../assets/logo.jpg" alt class="logo_img" />
                <span>运动管理平台</span>
            </div>
            <el-button type="info" @click="logout">安全退出</el-button>
        </el-header>
        <!-- //主体 -->
        <el-container>
            <!-- //侧边栏 -->
            <el-aside :width="isCollapse ?'64px':'200px'">
                <div class="toggle-button" @click="toggleCollapase">|||</div>
                <el-menu                
                    background-color="#545c64"
                    text-color="#fff"
                    active-text-color="#409eff"
                    unique-opened :collapse="isCollapse"
                    :collapse-transition="false"
                    :router="true"
                    :default-active="activePath">
                    <!-- 一级菜单 -->
                    <el-submenu :index="item.id+''" v-for="item in menuList" :key="item.id">
                        <template slot="title">
                            <i :class="iconsObject[item.id]"></i>
                            <span>{{item.title}}</span>
                        </template>
                        <!-- 二级菜单 -->
                        <el-menu-item :index="it.path" v-for="it in item.slist" :key="it.id" @click="saveNavState(it.path)">
                            <template slot="title">
                                <i :class="iconsObject[it.id]"></i>
                                <span>{{it.title}}</span>
                            </template>
                        </el-menu-item>
                    </el-submenu>
                </el-menu>
            </el-aside>
            <!-- //主体内容 -->
            <el-main>
                 <!--路由占位符-->
                <router-view></router-view>
            </el-main>
        </el-container>
    </el-container>
</template>

<script>
export default {
    data(){
        return{
            menuList: [],
            isCollapse:false,
            iconsObject: {
              '100':'iconfont iconguanliyuan',
              '200':'iconfont iconyundong',
              '101':'iconfont icondenglu',
              '102':'iconfont iconmima',
              '103':'iconfont iconyundong',
              '104':'iconfont iconshangpin',
              '201':'iconfont iconshu',
              '202':'iconfont iconqialuli',
              '203':'iconfont iconshiwu',
              '204':'iconfont icondenglu',
            },
            // 被激活的连接
            activePath:'/welcome',
        }
    },
    // 类似onload
    created() {
        //查询menuList，那么就要在data中绑定数据
        this.getMenuList();
        this.activePath = window.sessionStorage.getItem('activePath');// 取出session里的访问路径
    },
    methods:{
        logout(){
            window.sessionStorage.clear();
            this.$router.push("/login");
        },
       async getMenuList(){
            const {data:res} = await this.$http.get("menus");
            console.log(res.menus);
            if( res.flag != 200) return this.$message.error("操作失败！！！");
            this.menuList = res.menus;
        },
        // 切换菜单折叠与展开
        toggleCollapase(){
        
            this.isCollapse = !this.isCollapse;
        },
        saveNavState(activePath){
            window.sessionStorage.setItem('activePath',activePath);
             this.activePath = activePath;// 给点击的菜单添加高亮
        }
    },
}
</script>

<style lang="less" scoped>
//布局样式
.home-container {
  height: 100%;
}

// 头样式
.el-header {
    background-color: #373d41;

    display: flex;
    justify-content: space-between;// 左右贴边
    padding-left: 0%;// 左边界
    align-items: center;// 水平居中
    color: #fff;
    font-size: 20px;
    > div { //左侧div加布局
        display: flex;
        align-items: center;
        span {
            margin-left: 15px;
        }
    }
}
// 侧边栏样式
.el-aside {
  background-color: #333744;
  .el-menu{
    border-right: none;// 对其右边框
  }
}
// 主体样式
.el-main {
  background-color: #eaedf1;
}

.logo_img{ 
    width: 55px;
    height: 55px;
}
.toggle-button{
    background-color:#4A5064;
    font-size: 10px;
    line-height: 24px;
    color:#fff;
    text-align: center;
    letter-spacing: 0.2em;
    cursor: pointer;// 显示鼠标指针为：小手
}
</style>