<template>
  <div class="app-container">
    <div class="background-image"></div> <!-- 添加背景图片容器 -->
    
    <div class="fullscreen-image" :class="{ 'hidden': isScrolled }">
      <img src="./firstout.jpg" alt="Fullscreen Image" />
      <div class="center-button" @click="enterMainPage">
        <div class="button-content">
          <span class="button-text">Tongji OJ</span>
        </div>
      </div>
    </div>
    
    <div class="content-container" :class="{ 'visible': isScrolled }">
      <NavBar></NavBar>
      <div class="content-app">
        <transition name="fadeInUp" mode="out-in">
          <router-view></router-view>
        </transition>
        <div class="footer">
          <p>制作人：计科导论第九小组</p>
        </div>
      </div>
      <BackTop></BackTop>
    </div>
  </div>
</template>

<script>
import { mapActions, mapState } from 'vuex'
import NavBar from '@oj/components/NavBar.vue'

export default {
  name: 'app',
  components: {
    NavBar
  },
  data () {
    return {
      version: process.env.VERSION,
      isScrolled: false
    }
  },
  created () {
    try {
      document.body.removeChild(document.getElementById('app-loader'))
    } catch (e) {
    }
  },
  mounted () {
    this.getWebsiteConfig()
  },
  methods: {
    ...mapActions(['getWebsiteConfig', 'changeDomTitle']),
    enterMainPage () {
      this.isScrolled = true
    }
  },
  computed: {
    ...mapState(['website'])
  },
  watch: {
    'website' () {
      this.changeDomTitle()
    },
    '$route' () {
      this.changeDomTitle()
    }
  }
}
</script>

<style lang="less">
* {
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
}

a {
  text-decoration: none;
  background-color: transparent;
  &:active, &:hover {
    outline-width: 0;
  }
}

@media screen and (max-width: 1200px) {
  .content-app {
    margin-top: 160px;
    padding: 0 2%;
  }
}

@media screen and (min-width: 1200px) {
  .content-app {
    margin-top: 80px;
    padding: 0 2%;
  }
}

.footer {
  margin-top: 20px;
  margin-bottom: 10px;
  text-align: center;
  font-size: small;
}

.fadeInUp-enter-active {
  animation: fadeInUp .8s;
}

.app-container {
  position: relative;
  height: 100vh;
  overflow-y: auto;
}

/* 新增悬浮背景图样式 */
.background-image {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-image: url('./firstout.jpg'); /* 替换为你的背景图路径 */
  background-size: cover; /* 确保背景图完全覆盖 */
  background-position: center; /* 居中显示背景图 */
  opacity: 0.5; /* 设置透明度为50% */
  z-index: 0; /* 确保在最底层 */
}

.fullscreen-image {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 1000;
  transition: opacity 0.5s ease;
  img {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }
  &.hidden {
    opacity: 0;
    pointer-events: none;
  }
}

.center-button {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 200px;
  height: 200px;
  border-radius: 50%;
  background-color: rgba(0, 0, 0, 0.5);
  cursor: pointer;
  display: flex;
  justify-content: center;
  align-items: center;
  transition: background-color 0.3s ease;
  &:hover {
    background-color: rgba(0, 0, 0, 0.7);
  }
  .button-content {
    text-align: center;
    .button-text {
      color: #fff;
      font-size: 24px;
      font-weight: bold;
    }
  }
}

.content-container {
  position: relative;
  z-index: 999;
  transition: opacity 0.5s ease;
  opacity: 0;
  &.visible {
    opacity: 1;
  }
}

.content-app {
  min-height: calc(100vh - 80px); /* 确保内容区域足够高，允许滚动 */
}
</style>