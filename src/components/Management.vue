<template>
  <div class="container">
    <Layout class="layout">
      <Sider class="side-bar" :style="{height: 'calc(100vh - 35px)', background: '#fff', overflow: 'auto'}" ref="side" :collapsed-width="78">
        <Menu active-name="1-2" theme="light" width="auto" @on-select="onSelect">
          <div class="user-icon">
            <div class="user-img">
              <img src="static/img/head.jpg">
            </div>
            <p>系 统 管 理 员</p>
          </div>
          <Submenu name="1">
            <template slot="title">
                <Icon type="ios-bookmarks-outline"></Icon>
                <span>用户管理</span>
            </template>
            <MenuItem name="myStore">所有用户列表</MenuItem>
            <MenuItem name="user">封禁用户列表</MenuItem>
          </Submenu>
        </Menu>
      </Sider>
      <Layout class="layout">
        <Header :style="{background: '#fff'}">
          <h2>{{activeTitle}}</h2>
        </Header>
        <Content class="content">
          <transition mode="out-in">
            <router-view></router-view>
          </transition>
        </Content>
        <Footer class="layout-footer-center">2019 &copy; 大宗商品交易平台</Footer>
      </Layout>
    </Layout>
  </div>
</template>

<script>
export default {
  name: 'Management',
  data () {
    return {
      activeTitle: '所有用户列表',
      bar: {
        'myStore': '所有用户列表',
        'user': '封禁用户列表',
      }
    };
  },
  methods: {
    onSelect (name) {
      this.activeTitle = this.bar[name];
      this.$router.push(`/Management/${name}`);
    }
  }
};
</script>

<style scoped>
.side-bar a{
  color: #232323;
}
.user-icon {
  height: 200px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.user-icon span {
  font-size: 96px;
}
.user-img {
  margin-bottom: 15px;
  width: 96px;
  height: 96px;
  border-radius: 48px;
  overflow: hidden;
}
.user-img img{
  width: 100%;
}
.content {
  margin: 15px;
  background-color: #fff;
  padding: 15px;
}
.layout-footer-center {
  padding: 0px 15px;
  padding-bottom: 15px;
  text-align: center;
}
</style>
