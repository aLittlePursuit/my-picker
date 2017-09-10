<template>
  <div>
    <div class="hint" @click="showPicker">点我展示</div>
    <div class="select">{{ selectValue }} {{ selectValueId }}</div>
  </div>
</template>

<script type="text/ecmascript-6">
  import Picker from 'better-picker'

  export default {
    data () {
      return {
        selectValue: '',
        selectValueId: '',
        data1: [],
        data2: []
      }
    },
    created () {
      this.data1 = [{text: '纪楠', value: '1'}, {text: '晴子', value: '2'}, {text: '纪楠', value: '3'}, {text: '晴子', value: '4'}]
      this.data2 = [{text: '纪楠', value: '5'}, {text: '晴子', value: '6'}, {text: '纪楠', value: '7'}, {text: '晴子', value: '8'}]
      this.initPicker()
    },
    methods: {
      /**
       * 初始化筛选器，以及定义点击确定的方法和值改变的方法
       */
      initPicker () {
        this.picker = new Picker({
          data: [this.data1, this.data2],
          selectedIndex: [0, 0],
          title: '分类选择'
        })
        // 点击确定的回调方法
        this.picker.on('picker.select', (selectedVal, selectedIndex) => {
          this.selectValue = this.data1[selectedIndex[0]].text + ' ' + this.data2[selectedIndex[1]].text
          this.selectValueId = this.data1[selectedIndex[0]].value + ' ' + this.data2[selectedIndex[1]].value
        })
        // 滚动时的回调方法
        this.picker.on('picker.change', (index, selectedIndex) => {
//          console.log(index)
//          console.log(selectedIndex)
          // 判断如果滚动的是第一列，则拿到滚动值的id给第二列重新赋值
          if (index === 0) {
            // 拿到第一列的选中的id值
            console.log(this.data1[selectedIndex].value)
            // 给第一列重新赋值
            this.data2 = [{text: '该', value: '9'}, {text: '该', value: '10'}, {text: '该', value: '11'}, {text: '该', value: '12'}]
            // 重新填写第一列的数组
            this.picker.refillColumn(1, this.data2)
          }
        })
      },
      showPicker () {
        this.picker.show()
      }
    }
  }
</script>

<style lang="scss">
  .hint {
    height: 34px;
    line-height: 34px;
    text-align: center;
    background: red;
    color: #fff;
  }
  .select {
    height: 44px;
    line-height: 44px;
    background: #000;
    color: #fff;
  }
</style>
