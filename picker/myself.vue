<template>
  <transition name="fade">
    <div class="better" v-show="isVisible">
      <div class="head-top">
        <div class="cancel" @click="cancelOptions">取消</div>
        <div class="title">标题</div>
        <div class="confirm" @click="confirmData(data1Id, data2Id)">确定</div>
      </div>
      <div class="choose-wrapper">
        <div class="col" ref="col1">
          <ul class="content">
            <li v-for="item in data1">
              {{ item.name }}
            </li>
          </ul>
        </div>
        <div class="col col2" ref="col2">
          <ul class="content">
            <li v-for="item in data2">
              {{ item.name }}
            </li>
          </ul>
        </div>
      </div>
    </div>
  </transition>
</template>

<script type="text/ecmascript-6">
  import BScroll from 'better-scroll'

  export default {
    data () {
      return {
        isVisible: true,
        col1ScrollY: 0,
        col1ScrollIndex: 0,
        col2ScrollY: 0,
        col2ScrollIndex: 0,
        data1: [],
        data2: [],
        data1Id: '',
        data2Id: ''
      }
    },
    created () {
      this.data1 = [{id: '1', name: '检验科1'}, {id: '2', name: '检验科2'}, {id: '3', name: '检验科3'}, {id: '4', name: '检验科4'}]
      this.data2 = [{id: '5', name: '检验科5'}, {id: '6', name: '检验科6'}, {id: '7', name: '检验科7'}, {id: '8', name: '检验科8'}]
      this.data1Id = this.data1[0].id
      this.data2Id = this.data2[0].id
      this.$nextTick(() => {
        this._initScroll()
      })
    },
    methods: {
      _initScroll () {
        this.col1Scroll = new BScroll(this.$refs.col1, {
          probeType: 3,
          bounce: false
        })
        this.col2Scroll = new BScroll(this.$refs.col2, {
          probeType: 3,
          bounce: false
        })
        const holdHeight = 36
        this.col1Scroll.on('scroll', (pos) => {
          this.col1ScrollY = Math.abs(Math.round(pos.y))
          this.col1ScrollIndex = Math.round(this.col1ScrollY / holdHeight)
//          console.log(this.col1ScrollIndex)
          this.data1Id = this.data1[this.col1ScrollIndex].id
          console.log(this.data1Id)
        })
        this.col2Scroll.on('scroll', (pos) => {
          this.col2ScrollY = Math.abs(Math.round(pos.y))
          this.col2ScrollIndex = Math.round(this.col2ScrollY / holdHeight)
//          console.log(this.col2ScrollIndex)
          this.data2Id = this.data2[this.col2ScrollIndex].id
          console.log(this.data2Id)
        })
      },
      confirmData (id1, id2) {
        console.log('id1:' + id1, 'id2:' + id2)
      },
      cancelOptions () {
        this.isVisible = false
      }
    }
  }
</script>

<style lang="scss" rel="text/scss">
  .better {
    position: fixed;
    left: 0;
    bottom: 0;
    width: 100%;
    z-index: 10000;
    .head-top {
      display: flex;
      height: 44px;
      line-height: 44px;
      background: #ccc;
      .cancel, .confirm {
        flex: 0 0 80px;
        text-align: center;
      }
      .title {
        flex: 1;
        text-align: center;
        font-size: 18px;
      }
    }
    .choose-wrapper {
      position: relative;
      display: flex;
      height: 176px;
      background: #fff;
      &:before, &:after {
        position: absolute;
        left: 0;
        content: '';
        width: 100%;
        height: 1px;
        background: #ccc;
      }
      &:before {
        top: 70px;
      }
      &:after {
         top: 106px;
      }
      .col {
        flex: 1;
        height: 100%;
        overflow: hidden;
        text-align: center;
        .content {
          padding-bottom: 140px;
          margin-top: 70px;
          li {
            line-height: 36px;
          }
        }
      }
      .col2 {
        /*background: orange;*/
      }
    }
  }
  .fade-enter-active, .fade-leave-active {
    transition: opacity .4s
  }
  .fade-enter, .fade-leave-to {
    opacity: 0
  }
</style>
