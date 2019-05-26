<template>
    <div class="pagination">
        <button>首页</button>
        <button>上一页</button>
        <button v-for="btn in pagebtns"
        @click="changeBtn(btn)" 
        :class="[{currentPage:btn===currentPage},'pagebtn']">
            {{btn}}
        </button>
        <button>下一页</button>


    </div>
</template>



<script>
import $ from 'jquery'
export default {
    data(){
        return{
            pagebtns:[1,2,3,4,5,'...'],
            currentPage:1
        }
    },
    methods:{
        changeBtn(page){
            if(typeof page!=Number){
                switch(page.target.innerText){
                    case '上一页':
                    $('button.currentPage').prev().click();//选中的那个按钮的前一个执行click事件
                    break;
                    case '下一页':
                    $('button.currentPage').next().click();//选中的那个按钮后一个执行click事件
                    break;
                    case '首页':
                    this.pagebtns=[1,2,3,4,5,'...'];
                    this.changeBtn(1)
                    break;
                    default:
                        break;  
                }
                return;
            }
            this.currentPage=page;
            if(page===this.pagebtns[4]){
                this.pagebtns.shift();//移除第一个元素。
                this.pagebtns.splice(4,0,this.pagebtns[3]+1);//在第4个位置插上一个元素。

            }else if(page===this.pagebtns[0]&& page!=1){
                //在数组的第一个位置增加一个元素。
                this.pagebtns.unshift(this.pagebtns[0]-1)
                this.pagebtns.splice(5,1)//表示在第5个位置删除1个元素
            }
        }

    }
    
}
</script>




<style scoped>

</style>

