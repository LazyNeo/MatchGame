<template>
  <!-- <div class="feildF">
    <div class="feildS">
      <div class="feildT">
        <div style="position:relative;overflow:hidden;height:100%;">

        </div>
      </div>
    </div>
  </div> -->
  <!-- <div class="feild-line feild-line-">
    <hexagon-component :p-msg="msg" :show-list.sync="showLists[0]" class="feild-item"></hexagon-component>
  </div> -->
  <div class="">
    <div v-for="(line, index) in feilds" :class="'feild-line feild-line-' + (index + 1)">
      <hexagon-component :title="'x:' + index + ',y:' + index2"  v-for="(item,index2) in feilds[index]" v-bind:class="{'non-active' : !item.active}" :active.sync="item.active" :show-list.sync="item.showList" :init-item="item.initItem"  :x="index" :y="index2" @turn="turnItem"></hexagon-component>
    </div>
  </div>
</template>

<script>
import Vue from 'vue'
export default {
  data () {
    return {
      initPosition: {
        x: 3,
        y: 3
      },
      showLists: [
        [false, true, false, true, false, true],
        [false, true, false, true, false, true],
        [false, true, false, false, true, false],
        [false, true, false, false, true, false]
      ],
      active: [[true]],
      feilds: [],
      feildsShow: {
        a2_2: {
          active: false
        }
      }
    }
  },
  props: {
  },
  methods: {
    initFeild: function () {
      // let onePipe = [false, false, false, true, false, false]
      // let twoPipe = [false, true, false, false, true, true]
      let threePipe = [false, true, false, true, false, true]
      let level = 3
      let feildsT = this.feilds
      for (let i = level, index = 0; i <= 2 * level - 1; i++, index++) {
        feildsT[index] = []
        for (let j = 0; j < i; j++) {
          feildsT[index][j] = {}
          feildsT[index][j].showList = threePipe
          feildsT[index][j].border = this.getBorder(index, j, level)
          feildsT[index][j].active = false
        }
      }
      if (level > 1) {
        for (let i = 2 * level - 2, index = level; i >= level; i--, index++) {
          feildsT[index] = []
          for (let j = 0; j < i; j++) {
            feildsT[index][j] = {}
            feildsT[index][j].showList = threePipe
            feildsT[index][j].border = this.getBorder(index, j, level)
            this.feildsShow['a' + index + '_' + j] = {
              active: false
            }
          }
        }
      }
      feildsT[this.initPosition.x][this.initPosition.y].active = true
      return feildsT
    },
    getBorder: function (x, y, level) {
      let border = [{}, {}, {}, {}, {}, {}]
      let TL = {}
      let TR = {}
      let ML = {}
      let MR = {}
      let BL = {}
      let BR = {}
      // 中部
      ML.x = (y - 1 < 0) ? -1 : x
      ML.y = (y - 1 < 0) ? -1 : y - 1
      MR.x = (y + 1 > level + x - 1) ? -1 : x
      MR.y = (y + 1 > level + x - 1) ? -1 : y + 1
      if (x < level - 1) {
        // 顶部
        TL.x = (x - 1 < 0 || y - 1 < 0) ? -1 : x - 1
        TL.y = (x - 1 < 0 || y - 1 < 0) ? -1 : y - 1
        TR.x = (x - 1 < 0 || y > level + x - 2) ? -1 : x - 1
        TR.y = (x - 1 < 0 || y > level + x - 2) ? -1 : y
        // 底部
        BL.x = (x + 1 > 2 * level) ? -1 : x + 1
        BL.y = (x + 1 > 2 * level) ? -1 : y
        BR.x = (x + 1 > 2 * level || y + 1 > level + x) ? -1 : x + 1
        BR.y = (x + 1 > 2 * level || y + 1 > level + x) ? -1 : y + 1
      } else if (x === level - 1) {
        // 顶部
        TL.x = (x - 1 < 0 || y - 1 < 0) ? -1 : x - 1
        TL.y = (x - 1 < 0 || y - 1 < 0) ? -1 : y - 1
        TR.x = (x - 1 < 0 || y > level + x - 2) ? -1 : x - 1
        TR.y = (x - 1 < 0 || y > level + x - 2) ? -1 : y
        // 底部
        BL.x = (x + 1 > 2 * level || y - 1 < 0) ? -1 : x + 1
        BL.y = (x + 1 > 2 * level || y - 1 < 0) ? -1 : y - 1
        BR.x = (x + 1 > 2 * level || y > 2 * level - x) ? -1 : x + 1
        BR.y = (x + 1 > 2 * level || y > 2 * level - x) ? -1 : y
      } else {
        // 顶部
        TL.x = (x - 1 < 0) ? -1 : x - 1
        TL.y = (x - 1 < 0) ? -1 : y
        TR.x = (x - 1 < 0 || y + 1 > 2 * level - x + 1) ? -1 : x - 1
        TR.y = (x - 1 < 0 || y + 1 > 2 * level - x + 1) ? -1 : y + 1
        // 中部
        ML.x = (y - 1 < 0) ? -1 : x
        ML.y = (y - 1 < 0) ? -1 : y - 1
        MR.x = (y + 1 > 2 * level - x) ? -1 : x
        MR.y = (y + 1 > 2 * level - x) ? -1 : y + 1
        // 底部
        BL.x = (x + 1 > 2 * level - 2 || y - 1 < 0) ? -1 : x + 1
        BL.y = (x + 1 > 2 * level - 2 || y - 1 < 0) ? -1 : y - 1
        BR.x = (x + 1 > 2 * level - 2 || y > 2 * level - x - 1) ? -1 : x + 1
        BR.y = (x + 1 > 2 * level - 2 || y > 2 * level - x - 1) ? -1 : y
      }
      border[0] = TL
      border[1] = TR
      border[2] = MR
      border[3] = BR
      border[4] = BL
      border[5] = ML
      return border
    },
    activeItem: function (fromState, x, y) {
      let item = this.feilds[x][y]
      // console.log(item, x, y)
      if (item.active === true && fromState !== -1) {
        // 已经是激活态
        return
      }
      if (fromState !== -1 && item.showList[(fromState + 3) % 6] === false) {
        // 未连接
        return
      }
      // let feildsT = this.feilds
      item.active = true
      let row = this.feilds[x]
      row[y] = item
      Vue.set(this.feilds, x, row)
      // this.feilds = feildsT
      console.log('delight x: ' + x + 'y: ' + y)
      for (let i = 0; i < 6; i++) {
        if (item.border[i].x === -1 || item.showList[i] === false) {
          continue
        }
        let next = this.feilds[item.border[i].x][item.border[i].y]
        if (next.showList[(i + 3) % 6] === false || next.active === true) {
          // 下一个控件没有连接本控件或者下一个控件已经被点亮
          continue
        }
        this.activeItem(i, item.border[i].x, item.border[i].y)
      }
    },
    putOffAll () {
      let feild = this.feilds
      for (let i = 0, length = this.feilds.length; i < length; i++) {
        for (let j = 0, length2 = this.feilds[i].length; j < length2; j++) {
          // debugger
          if (!feild[i][j].active) {
            continue
          }
          if (i === this.initPosition.x && j === this.initPosition.y) {
            continue
          }
          let row = feild[i]
          row[j].active = false
          Vue.set(this.feilds, i, row)
        }
      }
    },
    turnItem (x, y) {
      this.putOffAll()
      let showList = this.feilds[x][y].showList
      let newList = []
      for (let i = 0; i < showList.length; i++) {
        newList[(i + 1) % 6] = showList[i]
      }
      showList = newList
      let row = this.feilds[x]
      row[y].showList = showList
      Vue.set(this.feilds, x, row)
      this.activeItem(-1, this.initPosition.x, this.initPosition.y)
      // window.console.log(showList)
    }
  },
  created: function () {
    this.feilds = this.initFeild()
  },
  ready: function () {
  },
  components: {
    'hexagon-component': require('./Hexagon.vue')
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
    .feildF, .feildS, .feildT{
        width: 400px;
        height: 500px;
        overflow: hidden;
        visibility: hidden;
    }
    .feildF{
        background-color: yellow;
        transform: rotate(120deg);
        float: left;
        .feildS{
            background-color: red;
            transform: rotate(-60deg);
            .feildT{
              background-color: #112233;
              transform: rotate(-60deg);
              visibility: visible;
            }
        }
    }
    .feild-line{
      width: 1000px;
      height: 125px;
      position: absolute;;
      border: 1px solid red;
      visibility: hidden;
    }
    .feild-item{
      float: left;
      margin-left: 72px;
    }
    .feild-line-1{
      left:100px;
    }
    .feild-line-2{
      top:94px;
      left:50px;
    }
    .feild-line-3{
      top:180px;
      left:0px;
    }
    .feild-line-4{
      top:266px;
      left:50px;
    }
    .feild-line-5{
      top:351px;
      left:100px;
    }
</style>
