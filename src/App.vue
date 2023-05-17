<template>
  <div class="name">{{ arr[index] }}</div>
  <div>
    <el-button
      type="primary"
      size="large"
      @click="() => (start ? stop() : run())"
      >{{ start ? '停止' : '开始' }}</el-button
    >
    <el-button type="default" size="large" @click="clear">清空</el-button>
  </div>
  <div style="padding-top: 20px">
    <el-input-number v-model="speed" :step="5"></el-input-number>
  </div>
  <div class="list">
    <div v-for="v in list" :key="v">
      {{ v }}
    </div>
  </div>
</template>

<script setup>
import { ref, reactive } from 'vue'
const data = [
  '林慧慧',
  '刘晓慧',
  '来桦琳',
  '陶金',
  '马文昌',
  '李娜',
  '余哲坤',
  '林小宜',
  '陶文杰',
  '石倩',
  '饶瑶',
  '方威',
  '汪婷婷',
  '陈晨晨',
  '赵柳春',
  '贺晓桐',
  '王婧',
  '张莹',
  '方超',
  '蔡畅',
  '孙颖',
  '王佳雯',
  '彭学瑞',
  '张凤灵',
  '吴婷',
  '祁海强',
  '叶丽君',
  '周鹏飞',
  '唐采虹',
  '陈小莉',
  '周瑾瑜',
  '来金明',
  '裘梦娜',
  '金其锋',
  '王蓉',
  '陈如颖',
  '汪琦',
  '舒伦',
  '叶倩雯',
  '张玲',
  '胡佳佳',
  '陈云多',
  '刘承林',
  '孙玉峰',
  '朱思妍',
  '孙洪轩',
  '李梦',
  '郭佳琳',
  '林微微',
  '孙许哲男',
  '章玉婷',
  '来娜',
  '章青青',
  '黄渊',
  '傅晓滨',
  '叶志强',
  '吴咏蕾',
  '王君',
  '王海青',
  '匡宇',
  '孙易劼',
  '许天一',
  '王梦倩',
  '程瑶',
  '刘婷婷',
  '李云凤',
  '邢肖平',
  '魏婉',
  '项淑雅',
  '傅婷婷',
  '都琳琳',
  '徐冰萤',
  '葛燕楠',
  '屠逸妮',
  '范海瑞',
  '吴烨彪',
  '蒋雪',
  '金雪平',
  '钟文涛',
  '童蘩茜',
  '吴慧敏',
  '吴敏',
  '陈晓',
  '廖雅茜',
  '孙熠',
  '王婷',
  '朱佳培',
  '方洁',
  '石旭',
  '姜楠',
  '武晨杰',
  '严明亮',
  '彭方波',
  '马迪',
  '祁金月',
  '杨灵丹',
  '应佳宏',
  '官宵宇',
  '刘倩',
  '马独秀',
  '彭芳森',
  '张倩',
  '宿叶菁菁',
  '赵赞瑜',
  '陆明嘉',
  '袁其凯',
  '杨涛',
  '邵枝林',
  '殷大强',
  '陈建兰',
  '郑松松',
  '徐晨',
  '孙睿通',
  '赵剑梅',
  '陈冰冰',
  '徐书晴',
  '苏汇婷',
  '王羽姗',
  '冯晨曦',
  '唐琳婧',
  '邱圣娟',
  '王梦乾',
  '毕霁洁',
  '李一涛',
  '高欣逸',
  '余磊',
  '陈春富',
  '朱莉薇',
  '张诗憧',
  '钱立钧',
  '李政锋',
  '王涛',
  '张亚飞',
  '王振新',
  '常立丰',
  '上官吴忧',
  '杜佳坤',
  '任兴',
  '黎柏沙',
  '苑维鑫',
  '奚陈红',
  '罗赐安',
  '卢韵田',
  '王欢',
  '潘悦',
  '洪念海',
  '曾天佑',
  '吴安吉',
  '韩凯',
  '陈卓',
  '范浩杰',
  '陈华',
  '何浩源',
  '姜玉',
  '聂仁杰',
  '许文倩',
  '郭洪吉',
  '陈水华',
  '卞少杰',
  '阮靖杰'
]

const list = reactive(JSON.parse(localStorage.list ?? null) ?? [])
const arr = reactive(data.filter((v) => !list.includes(v)))
const backet = []

const start = ref(false)
const speed = ref(50)
const index = ref(-1)
const getNewOne = ref(false)

let timer = null
const run = () => {
  start.value = true
  if (getNewOne.value) {
    arr.splice(index.value, 1)
    getNewOne.value = false
  } else if (index.value > -1) {
    const k = arr.splice(index.value, 1)[0]
    backet.push(k)
  }
  if (!arr.length) {
    arr.push(...backet)
    backet.length = 0
  }
  if (!arr.length) return
  const r = parseInt(Math.random() * arr.length)
  index.value = r
  timer = setTimeout(run, speed.value)
}

const stop = () => {
  clearTimeout(timer)
  start.value = false
  getNewOne.value = true
  list.push(arr[index.value])
  localStorage.list = JSON.stringify(list)
}

const clear = () => {
  getNewOne.value = false
  localStorage.list = null
  list.length = 0
}

document.body.addEventListener('keyup', (e) => {
  if (e.keyCode === 32) {
    start.value ? stop() : run()
  }
})
</script>

<style>
body {
  padding: 0;
  margin: 0;
  overflow: hidden;
}
#app {
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  background: url(./assets/12131231313.jpg);
}
.name {
  margin-top: 30px;
  font-size: 68px;
  height: 70px;
  margin-bottom: 40px;
}
.list {
  position: absolute;
  right: 40px;
  top: 50px;
  height: 50px;
  line-height: 50px;
  font-size:32px;
}
</style>
