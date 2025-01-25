<template>
  <el-row type="flex" justify="center" class="header">
    <el-col :span="20" class="nav-content">
      <router-link to="/" class="logo" style="color: #409eff;margin-right: 10px;">
        <img src="@/icons/logo.png">
      </router-link>
      <el-input
        ref="SearchInput"
        v-model.trim="searchInput"
        size="small"
        class="nav-search"
        placeholder="搜索课程"
        @keyup.enter.native="search"
      >
        <el-button slot="append" icon="el-icon-search" circle style="color: #fff" @click="search" />
      </el-input>
      <div class="nav-right">
        <el-button
          v-if="user===null || Object.keys(user).length===0"
          type="primary"
          round
          :underline="false"
          class="nav-item"
          @click="$login"
        >登录</el-button>
        <el-dropdown v-else class="avatar-container" trigger="hover">
          <div class="avatar-wrapper">
            <el-avatar :size="32" :src="user.avatar" fit="contain" />
            <span class="name">{{ user.nickname || '' }}</span>
            <i class="el-icon-caret-bottom" />
          </div>
          <el-dropdown-menu slot="dropdown" class="user-dropdown">
            <router-link :to="{name: 'Profile'}">
              <el-dropdown-item icon="el-icon-user">
                个人中心
              </el-dropdown-item>
            </router-link>
            <router-link :to="{name: 'Order'}" class="nav-item el-link el-link--default">
              <el-dropdown-item icon="el-icon-user">
                我的订单
              </el-dropdown-item>
            </router-link>
            <el-dropdown-item divided icon="el-icon-switch-button" @click.native="logout">
              退出登录
            </el-dropdown-item>
          </el-dropdown-menu>
        </el-dropdown>
      </div>
    </el-col>
  </el-row>
</template>

<script>
import { mapGetters } from 'vuex'

export default {
  name: 'Header',
  data() {
    return {
      loginFormVisible: false,
      searchInput: ''
    }
  },
  computed: {
    ...mapGetters(['user'])
  },
  methods: {
    async logout() {
      try {
        await this.$store.dispatch('user/logout')
        await this.$router.push({ name: 'Index' })
        // eslint-disable-next-line no-empty
      } catch (ignore) { }
    },
    search() {
      if (!this.searchInput) {
        this.$refs.SearchInput.focus()
        return
      }
      // const { href } = this.$router.resolve({
      //   name: 'SearchByKeyword',
      //   params: { title: this.searchInput }
      // })
      // window.open(href, '_blank')
      this.$router.push(
        {
          name: 'SearchByKeyword',
          params: { title: this.searchInput }
        }
      )
      this.searchInput = ''
    }
  }
}
</script>

<style lang="scss">
.nav-search {
  width: 20vw;
  position: absolute;
  right: 20vw;
  transform: translateX(50%);

  .el-input__inner {
    border-radius: 50px;
    border-color: #9d9c9c;
  }

  .el-input-group__append {
    background-color: rgba($color: #fff, $alpha: 0);
    border: none;
    position: relative;
    right: 40px;
  }

  .el-input-group__append .el-icon-search {
    color: #7b7b7b;
  }
}
</style>

<style scoped lang="scss">
.header {
  height: 60px;
  background-color: #fff;

  .nav-content {
    position: relative;
    display: flex;
    align-items: center;
  }

  .nav-item {
    margin: 0 20px;
    font-size: 16px;
    width: 80px;
    height: 32px;
    line-height: 8px;

    &:first-child {
      margin-left: 0;
    }

    &:last-child {
      margin-right: 0;
    }
  }

  .nav-right {
    position: absolute;
    right: 0;
    display: flex;
    align-items: center;
  }

  .avatar-container {
    .avatar-wrapper {
      cursor: pointer;
      display: flex;
      align-items: center;

      &:hover {
        .el-icon-caret-bottom {
          transform: rotate(180deg);
        }
      }

      .name {
        margin: 0 5px 0 10px;
      }

      .el-icon-caret-bottom {
        transition: all .7s;
        font-size: 14px;
      }
    }
  }
}

@media screen and (max-width: 700px) {
  .nav-search {
    display: none;
  }
}
</style>
