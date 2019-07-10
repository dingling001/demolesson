<template>
  <div>
    <p>{{result}} </p>
    <div>
      <div class="span" v-for="(i,index) in dataList" :data-key="i">{{ i }}
        <button @click="del(index)">删除</button>
      </div>
    </div>
    <div>
      <button @click="changedata">改变</button>
      <img v-show="show" src="../../static/998023-20180519212338609-1617459354.png" alt="">
      <button @click="show=!show">图片diff</button>

    </div>
    <input type="text" @input="" @keydown="debounces">
    <button @click="throttles">节流
      <button @click="conun">{{a}}</button>
    </button>
  </div>
</template>

<script>
  var timer = null;
  var lastTime = null;
  export default {
    name: 'test',
    data() {
      return {
        result: '131',
        dataList: [1, 2, 3, 4, 5],
        dataList1: [4, 1, 3, 5, 2], // 数据位置替换
        show: false,
        a:0
      }
    },
    created() {
      let a = ['1', '2', '3'].map((item, index) => {
        return parseInt(item, index)
      })
      console.log(a)

    },
    methods: {
      changedata() {
        this.dataList = this.dataList1
      },
      del(index) {
        this.dataList.splice(index, 1)
      },
      debounces: function () {
        let that = this
        if (timer) {
          clearTimeout(timer)
        }
        timer = setTimeout(function () {
          console.log('输入')
          timer = undefined;
        }, 2000)
      },
      throttles: function () {
        let that = this
        let now = +new Date();
        if (lastTime && lastTime - now < 2000) {
          clearTimeout(timer)
        }
        timer = setTimeout(function () {
          console.log('点击')
          lastTime = +new Date()
        }, 200)
      },
      debounce(fn) {
        let timeout = null; // 创建一个标记用来存放定时器的返回值
        return function () {
          clearTimeout(timeout); // 每当用户输入的时候把前一个 setTimeout clear 掉
          timeout = setTimeout(() => { // 然后又创建一个新的 setTimeout, 这样就能保证输入字符后的 interval 间隔内如果还有字符输入的话，就不会执行 fn 函数
            fn.apply(this, arguments);
          }, 3000);
        };
      },
      throttle(fn) {
        let canRun = true; // 通过闭包保存一个标记
        return function () {
          if (!canRun) return; // 在函数开头判断标记是否为true，不为true则return
          canRun = false; // 立即设置为false
          setTimeout(() => { // 将外部传入的函数的执行放在setTimeout中
            fn.apply(this, arguments);
            // 最后在setTimeout执行完毕后再把标记设置为true(关键)表示可以执行下一次循环了。当定时器没有执行的时候标记永远是false，在开头被return掉
            canRun = true;
          }, 500);
        };
      },
      conun() {
        this.a+=1;
        console.log(this.a)
      },
      // sayHi() {
      //   this.debounce(this.test())
      // },
      test() {
        console.log('防抖成功');
      }
    }
  }
</script>

<style scoped>
  div {
    padding: 20px;
    text-align: center;
    font-size: 25px;
  }

  .span {
    padding: 0 5px;
  }
</style>
