<script setup lang="ts">
import { ref } from 'vue'
import useSettingStore from './settingstore'
import MySwitch from '../layout/MySwitch.vue'
import Config from '../utils/config'
import ServerHttp from '../aliapi/server'

const settingStore = useSettingStore()
const cb = (val: any) => {
  settingStore.updateStore(val)
}

const verLoading = ref(false)

const handleCheckVer = () => {
  verLoading.value = true
  ServerHttp.CheckUpgrade(true).then(() => {
    verLoading.value = false
  })
}
</script>

<template>
  <div class="settingcard">
    <div class="appver"></div>
    <!--<div class="appver">-->
    <!--  <a-button type="outline" size="mini" tabindex="-1" :loading="verLoading" @click="handleCheckVer">检查更新</a-button>-->
    <!--</div>-->
    <div class="settinghead">外观</div>
    <div class="settingrow">
      <button class="theme-button system-theme" @click="cb({ uiTheme: 'system' })">
        <span></span>
      </button>
      <button class="theme-button light-theme" @click="cb({ uiTheme: 'light' })">
        <span></span>
      </button>
      <button class="theme-button dark-theme" @click="cb({ uiTheme: 'dark' })">
        <span></span>
      </button>
    </div>
    <div class="settingspace"></div>
    <div class="settingspace"></div>
    <div class="settinghead">关闭窗口</div>
    <div class="settingrow">
      <MySwitch :value="settingStore.uiExitOnClose" @update:value="cb({ uiExitOnClose: $event })"></MySwitch>
      <a-popover position="right">
        <i class="iconfont iconbulb" />
        <template #content>
          <div>
            默认：<span class="opred">关闭</span>
            <hr />
            默认是点击窗口上的关闭按钮时<br />最小化到托盘，继续上传/下载<br /><br />开启此设置后直接彻底退出小白羊程序
          </div>
        </template>
      </a-popover>
    </div>
  </div>
</template>

<style scoped>
.settinghead {
  display: flex;
  margin-right: 20px;
}
.theme-button {
  display: flex;
  justify-content: center;
  border: none;
  cursor: pointer;
  color: #fff;
  padding: 15px 40px;
  border-radius: 5px;
  margin-right: 30px;
  background-size: cover;
  background-position: center center;
  background-repeat: no-repeat;
  flex-direction: row;
  /*align-items: flex-start;*/
}

.theme-button span {
  display: flex;
  margin-top: 10px;
}

/*.settingrow {*/
/*  display: flex;*/
/*}*/

.system-theme {
  background-image: url('../../electron/assets/follow_button.png');
  background-color: #888;
}

.light-theme {
  background-image: url('../../electron/assets/light_button.png');
  background-color: #eee;
}

.dark-theme {
  background-image: url('../../electron/assets/dark_button.png');
  background-color: #333;
}
.settinghead, .settingrow {
  display: inline-flex;
  width: 50%;
  padding: 10px;
  box-sizing: border-box;
  vertical-align: top;
}

.appver {
  margin-bottom: 0.5em;
  font-weight: 600;
  font-size: 20px;
  line-height: 1.4;
  text-align: center;
}
</style>

