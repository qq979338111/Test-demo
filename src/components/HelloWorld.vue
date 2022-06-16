<template>
  <div class="Thebody">
    <div><img src="../assets/banner1.jpg" alt="" class="Mybanner" /></div>
    <!--下面那个是建模的软件 -->
    <div>
      <h3>1.下面那个是建模的软件？</h3>
      <div class="Soft">
        <div class="Soft-box" v-for="(item, index) in SoftData">
          <el-radio v-model="soft" :label="item.label" @change="GetSoftData(soft)">{{ item.label }}</el-radio>
        </div>
      </div>
    </div>

    <!-- 以下那个游戏是3d游戏 -->
    <div>
      <h3>2.以下哪个游戏是3d游戏？</h3>
      <div class="Soft">
        <div class="Soft-box" v-for="(item, index) in GameDATA">
          <el-radio v-model="game" :label="item.label" @change="GetGameData(game)">{{ item.label }}</el-radio>
        </div>
      </div>
    </div>

    <!-- 下面那个建模你觉得最难 -->
    <div>
      <h3>3.下面那个建模你觉得最难？</h3>
      <div class="Model">
        <div v-for="(item, index) in Model" class="Model-box">
          <el-radio v-model="model" :label="item.label" @change="GetModelData(model)">{{ item.label }}</el-radio>
          <div>
            <img class="Model-img" :src="require(`../assets/model${index}.gif`)" />
          </div>
        </div>
      </div>
    </div>
    <!-- 年龄 -->
    <div>
      <h3>* 4.年龄？</h3>
      <div class="Soft">
        <div v-for="(item, index) in Age" class="Age-box">
          <el-radio v-model="age" :label="item.label" @change="GetAge(age)">{{ item.label }}</el-radio>
        </div>
      </div>
    </div>
    <!-- 您的手机号码是 -->
    <div>
      <h3>* 5.您的手机号码是？</h3>

      <el-form :model="phoneNum" :rules="Rules">
        <el-form-item prop="number">
          <el-input v-model="phoneNum.number" class="Myinput" placeholder="输入手机号码（已做二级加密）"> </el-input>
        </el-form-item>
      </el-form>
    </div>

    <div>
      <el-row>
        <el-button class="Mybutton" type="info" @click="submitData()">提交</el-button>
      </el-row>
    </div>

    <div class="Carousel">
      <h4>最新报名用户</h4>

      <div class="content-flex" v-for="(item, index) in CarouselData">
        <p class="">{{ item.name }}</p>
        <p class="content-data">{{ item.phone }}</p>
        <p class="">{{ item.time }}</p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'HelloWorld',
  data () {


    var checkMobile = (rule, value, cb) => {
      //验证手机号的正则表达式
      const regMobile = /^1(3[0-9]|4[5,7]|5[0,1,2,3,5,6,7,8,9]|6[2,5,6,7]|7[0,1,7,8]|8[0-9]|9[1,8,9])\d{8}$/;

      if (regMobile.test(value)) {
        return cb()
      }
      cb(new Error('请输入正确的手机号码'))
    }
    return {

      soft: '0'
      ,
      SoftData: [
        {
          label: 'Excel',

        },
        {
          label: '3Dsmax',

        },
        {
          label: 'Photoshp',

        },

      ],
      game: '0',
      GameDATA: [
        {
          label: '英雄联盟',

        },
        {
          label: '王者荣耀',

        },
        {
          label: '绝地求生',

        },
        {
          label: '以上都是',

        }
      ]
      ,
      model: '0',

      Model: [
        {
          label: '人物建模',

        },
        {
          label: '道具建模'
        }, {
          label: '场景建模'
        }
      ],
      age: '0',
      Age: [
        {
          label: '17岁以下',


        },
        {
          label: '18~20岁'
        }, {
          label: '21~24岁'
        }
      ]
      ,
      selectSoft: '',
      selectGame: '',
      selectModel: '',
      selectAge: '',

      phoneNum: {
        number: ''
      },//手机号码
      Rules: {

        number: [
          { required: false, messagge: '请输入手机', trigger: 'blur' }, { validator: checkMobile, trigger: 'blur' }

        ],
      },

      CarouselData: [
        {
          name: '周**',
          phone: '136****4126',
          time: '2分钟前'
        },
        {
          name: '吴**',
          phone: '138****6313',
          time: '7分钟前'
        },
        {
          name: '王**',
          phone: '159****2346',
          time: '16分钟前'
        },
        {
          name: '李**',
          phone: '189****5684',
          time: '25分钟前'
        },
        {
          name: '陈**',
          phone: '178****1342',
          time: '50分钟前'
        },

      ],


    }
  },
  methods: {
    GetSoftData (soft) {
      this.selectSoft = soft
      console.log('我是this.selectSoft', this.selectSoft);
    }
    , GetGameData (game) {
      this.selectGame = game
      console.log('我是 this.selectGame', this.selectGame);
    },
    GetModelData (model) {
      this.selectModel = model
      console.log('我是模型', this.selectModel);
    },
    GetAge (age) {
      this.selectAge = age
      console.log('我是年龄', this.selectAge);
    }

    ,
    submitData () {

      axios({
        method: 'post',
        url: 'http://124.71.73.195:7766/mt',
        data: {
          "formStr": this.selectSoft + ',' + this.selectGame + ',' + this.selectModel + ',' + this.selectAge,
          "phone": this.phoneNum,
          "empName": "傅庆发"
        },
        headers: { 'Content-Type': 'application/json' },

      }).then((res) => {
        if (res.data.code === 1) {
          alert('提交成功！')
        }
      });
    }
  }


}


</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.Thebody {
  margin: 0 auto;
  width: 1200px;
  background-color: rgb(255, 255, 255);
}
.Mybanner {
  width: 100%;
}
.Soft {
  display: flex;
  flex-direction: row;
  justify-content: start;
}
.Soft-box {
  position: relative;
  margin-left: 80px;
  left: -70px;
}
.Model {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}
.Model-box {
  position: relative;
  margin-left: 100px;
  left: -80px;
}
.Model-img {
  position: relative;
  top: 10px;
  width: 200px;
  height: 120px;
}
.Age-box {
  position: relative;
  margin-left: 130px;
  left: -130px;
}
.Myinput >>> .el-input__inner {
  width: 98%;
  height: 50px;
}
.Myinput.xrequired::before {
  content: '* ';
  color: red;
}
/deep/ .el-button--info {
  margin-top: 20px;
  width: 98%;
  color: #fff;
  background-color: #909399;
  border-color: #909399;
}
.Carousel {
  margin-top: 40px;
  width: 98%;
  background-color: #fdfdff;
  height: 340px;
  margin-bottom: 50px;
  text-align: center;
  border: 1px solid #8e8f91;
}
.content-flex {
  display: flex;
  flex-direction: row;
  justify-content: center;
}
.content-data {
  margin-left: 200px;
  margin-right: 200px;
}
</style>
