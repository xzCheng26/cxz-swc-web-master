<template>
  <div class="frameTop">
   <nav class="navbar navbar-static-top">
    <div class="navbar-head" style="width:220px;">
      <img src="../assets/image/log2.png">
    </div>
    <div class="navbar-head myhead">
      <button class="btn btn-default btn-xs uinfo-btn" type="button" :class="swatchcls" @click="swatch" title="切换菜单" style="margin-top:15px;"><i class="fa fa-bars"></i></button>
    </div>
    <ul class="navbar-right-my">
      <li>
        <a>
          <el-dropdown @command="handleCommand" menu-align='start'>
            <span class="avator" @mouseover="mover" @mouseout="mout">
              <i class="fa fa-user"></i> 用户 <span :class="userbtnstyle" class="glyphicon"></span>
            </span>
            <el-dropdown-menu slot="dropdown">
              <el-dropdown-item command="xgmm">修改密码</el-dropdown-item>
              <el-dropdown-item command="signout">退出登录</el-dropdown-item>
            </el-dropdown-menu>
          </el-dropdown>
        </a>
      </li>
    </ul>
	</nav>
  <userInfo ref="uinfo"></userInfo>
  <password ref="psd"></password>
  </div>
</template>

<script>
import {baseUrl} from '@/common/biConfig'
import $ from 'jquery'
import userInfo from '@/components/UserInfo'
import password from '@/components/Password'

export default {
  name: 'FrameTOp',
  data () {
    return {userbtnstyle:'glyphicon-menu-up',swatchcls:''}
  },
  components: {
		userInfo,password
	},
  methods:{
    mover:function(){
      this.userbtnstyle = "glyphicon-menu-down";
    },
    mout:function(){
      this.userbtnstyle = "glyphicon-menu-up";
    },
    swatch:function(){
      if(this.swatchcls === ''){
        this.swatchcls = 'swatchBtn';
        this.$parent.isShowMenu = false;
        window.setTimeout(()=>{
          $(".page-wrapper").css({"margin": "0", "width":"100%"})
          $(window).trigger("resize");
        }, 200);
      }else{
        this.swatchcls = '';
        this.$parent.isShowMenu = true;
        window.setTimeout(()=>{
          $(".page-wrapper").css({"margin": "0 0 0 220px", "width":"calc(100% - 220px)"});
           $(window).trigger("resize");
        }, 200);
      }
    },
    handleCommand:function(cmd){
        let ts = this;
        if(cmd === 'signout'){
          $.ajax({
            url:baseUrl + "frame/Logout.action",
            data:{},
            dataType:'json',
            xhrFields: {withCredentials: true},
            crossDomain: true,
            success:function(){
                ts.$notify.success({
                  title: '退出成功',
                  offset: 50
                });
               ts.$router.push('/');
            }
          });
        }else if(cmd === 'grxx'){
          this.$refs['uinfo'].show = true;
        }else if(cmd === 'xgmm'){
          this.$refs['psd'].show = true;
        }
    }
  }
}
</script>

<style scoped lang="less">
  @import '../style/mixin';
  .frameTop {
    height: 50px;
    background-color: black;
  }
  .navbar-head {
    float: left;
  }
  .navbar-right-my {
    float:right;
    a {
      display: inline-block;
      height: 50px;
    }
    .avator {
       display:inline-block;
       padding: 13px;
       font-size: 16px;
       color: white;
       &:hover {
          background-color: white;
          color: black;
       }
    }
  }
  @media (max-width: 512px) {
    .myhead {
      display: none;
    }
  }
  .swatchBtn {
    transform:rotate(90deg);
    -ms-transform:rotate(90deg); 	/* IE 9 */
    -moz-transform:rotate(90deg); 	/* Firefox */
    -webkit-transform:rotate(90deg); /* Safari å’Œ Chrome */
    -o-transform:rotate(90deg); 	/* Opera */
  }
  li {
    list-style-type: none;
  }
  ul {
    margin-bottom:0px;
  }
</style>
