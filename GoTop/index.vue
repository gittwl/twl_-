<template>
  <div class="GoTop" ref="top" @click="goTop">

  </div>
</template>

<script>
export default {
    name:'gotop',
    computed:{
        top(){
            return {
                timer:null,
            }
        }
    },
    methods:{
        //判断当前是否隐藏或显示GoTop按钮
        handleScrollx() {
            if(window.pageYOffset >= 30){
                this.$refs.top.style.display = "block";
            }else{
                this.$refs.top.style.display = "none";
            }
        },
        //返回顶部
        goTop(){
            cancelAnimationFrame(this.timer);
            const length = window.pageYOffset;
            const self = this;
            self.timer = requestAnimationFrame(function fn () {
                const oTop = document.body.scrollTop || document.documentElement.scrollTop;
                if (oTop > 0) {
                    document.body.scrollTop = document.documentElement.scrollTop = oTop - length/10;
                    self.timer = requestAnimationFrame(fn);
                } else {
                    cancelAnimationFrame(self.timer);
                }
            });
        }
    },
    mounted(){
        // 监听当前页面的rolly
        window.addEventListener('scroll',this.handleScrollx,true)
    },
    beforeDestroy(){
        window.removeEventListener('scroll',this.handleScrollx)
    }
}
</script>

<style scoped lang="less" >
    .GoTop{
        display: none;
        position: fixed;
        text-indent: -9999px;
        left: 50%;
        margin-left: 500px;
        bottom: 160px;
        width: 49px;
        height: 44px;
        background-position: -265px -47px;
        z-index: 0;
        background: url('./gotop.png') no-repeat -265px -47px;

        &:hover{
            background-position: -325px -47px;
            cursor: pointer;
        }
    }
</style>