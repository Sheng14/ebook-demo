<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App'
}
document.addEventListener('DOMContentLoaded', () => {
  const html = document.querySelector('html') // 获取根元素
  let fontSize = window.innerWidth / 10 // 设置尺寸
  fontSize = fontSize > 50 ? 50 : fontSize // 如果超过上限50通通计为50
  html.style.fontSize = fontSize + 'px' // 把属性值写到根元素中
})
</script>

<style>

</style>
