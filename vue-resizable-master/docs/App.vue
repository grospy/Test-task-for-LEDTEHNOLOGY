<template>
  <div>
    <div class="container">
      <vue-resizable class="resizable" ref="resizableComponent"
                     :dragSelector="dragSelector"
                     :active="handlers" :fit-parent="fit" :maximize="maximize"
                     :max-width="checkEmpty(maxW)" :max-height="checkEmpty(maxH)"
                     :min-width="checkEmpty(minW)" :min-height="checkEmpty(minH)"
                     :width="width" :height="height"
                     :left="left" :top="top"
                     @mount="eHandler"
                     @resize:move="eHandler" @resize:start="eHandler" @resize:end="eHandler"
                     @drag:move="eHandler" @drag:start="eHandler" @drag:end="eHandler" @maximize="eHandler"
      >
        <div class="block">
          <div class="drag-container-1"><span>T</span></div>
          <div class="table-container">
            <table>
          <div class="drag-container-5" style="z-index:101;  position: absolute; left:50%; top: 50%; width: 15px; height:15px; padding: 0px; border: 0px;" id="wrapper" align="left" >c</div>
            </table>
          </div>
          <div class="drag-container-2">B</div>
          
          <div class="drag-container-3" style="z-index:101;  position: absolute; left: 0px; top: 50%; width: 15px; height:30px; padding: 0px; border: 0px;" id="wrapper" align="left" >L</div>
          <div class="drag-container-4" style="z-index:101;  position: absolute; right: 0px; top: 50%; width: 15px; height:30px; padding: 0px; border: 0px;" id="wrapper" align="left" >R</div>
          
        </div>
      </vue-resizable>
    </div>
    <div class="container table-block">
      <div class="table-row">
        <div><h4>Обработчики:</h4></div>
        <span v-for="handler in ['r', 'rb', 'b', 'lb', 'l', 'lt', 't', 'rt']" :key="handler">
                    {{ handler }}:<input type="checkbox" v-model="handlers" :value="handler"/>
               </span>
      </div>
      <div class="table-row">
        <div class="table-cell">
          минимальнаяШирина:<input class="table-input" type="number" v-model.number="minW"/>
        </div>
        <div class="table-cell">
          максимальнаяШирина:<input class="table-input" type="number" v-model.number="maxW"/>
        </div>
      </div>
      <div class="table-row">
        <div class="table-cell">
          минимальнаяВысота:<input class="table-input" type="number" v-model.number="minH"/>
        </div>
        <div class="table-cell">
          максимальнаяВысота:<input class="table-input" type="number" v-model.number="maxH"/>
        </div>
      </div>
      <div class="table-row">
        <div class="table-cell">
          ширина:<input class="table-input" type="number" v-model.number="width"/>
        </div>
        <div class="table-cell">
          высота:<input class="table-input" type="number" v-model.number="height"/>
        </div>
      </div>
      <div class="table-row">
        <div class="table-cell">
          слева:<input class="table-input" type="number" v-model.number="left"/>
        </div>
        <div class="table-cell">
          сверху:<input class="table-input" type="number" v-model.number="top"/>
        </div>
      </div>
      <div class="table-row">
        <div class="table-cell">
          оставить в границе:<input type="checkbox" v-model.number="fit"/>
        </div>
        <div class="table-cell">
          максимизировать:<input type="checkbox" v-model.number="maximize"/>
        </div>
      </div>
      <div class="table-row" style="text-align: left;">
        <div class="table-cell" style="padding: 0 20px;width: 100%">
          lastEvent: {{ event }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import VueResizable from '../src/components/vue-resizable'

export default {
  name: "App",
  components: { VueResizable },
  data() {
    const tW = 150;
    const tH = 150;
    return {
      handlers: ['r', 'rb', 'b', 'lb', 'l', 'lt', 't', 'rt'],
      left: `calc( 50% - ${tW / 2}px)`, top: `calc(50% - ${tH / 2}px)`,
      height: tH, width: tW,
      maxW: 250, maxH: 250,
      minW: 100, minH: 100,
      fit: true, maximize: false, event: '',
      dragSelector: ".drag-container-1, .drag-container-2, .drag-container-3, .drag-container-4, .drag-container-5"
    };
  },
  methods: {
    eHandler(data) {
      this.width = data.width;
      this.height = data.height;
      this.left = data.left;
      this.top = data.top;
      this.event = data.eventName;
      if (data.eventName === 'maximize') {
        this.maximize = data.state;
      }
    },
    checkEmpty(value) {
      return typeof value !== "number" ? 0 : value;
    }
  },
  filters: {
    checkEmpty(value) {
      return typeof value !== "number" ? 0 : value;
    }
  }
}
</script>

<style scoped>
.block {
  height: 100%;
  width: 100%;
  background-color: aqua;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
</style>
<style>
body, html {
  height: 100%;
  width: 100%;
  margin: 0;
  padding: 0;
  overflow: auto;
}

.container {
  width: 500px;
  height: 500px;
  display: inline-block;
  border: 1px solid #dddddd;
  background: #ffffff;
  color: #333333;;
  float: left;
  margin: 10px;
}

#block1 {
  border: solid black 1px;
  height: 1000px;
  width: 1000px;
  display: inline-block;
  float: left;
}

.resizable {
  background-position: top left;
  width: 150px;
  height: 150px;
  padding: 0;
  border: 1px solid #003eff;
  background: #007fff;
  font-weight: normal;
  color: #ffffff;
  position: relative;
}

.table-block {
  display: table;
}

.table-row {
  display: table-row;
  text-align: center;
}

.table-cell {
  width: 50%;
  display: inline-block
}

.table-input {
  width: 50px
}

.drag-container-1, .drag-container-2 {
  width: 30%;
  height: 15px;
  background: red;
  color: white;
  text-align: center;
  cursor: pointer;
}
.drag-container-3 {
  width: 30%;
  height: 30px;
  background: red;
  color: white;
  text-align: center;
  cursor: pointer;
  align: left;
}
.drag-container-4 {
  width: 30%;
  height: 30px;
  background: red;
  color: white;
  text-align: center;
  cursor: pointer;
  align: right;
}
.drag-container-5 {
  width: 30%;
  height: 15px;
  background: red;
  color: white;
  text-align: center;
  cursor: pointer;
  align: center;
}


.table-container {
  flex: 1;
  width: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

</style>