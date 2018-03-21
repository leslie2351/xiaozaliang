
<style lang="less">
    @import "./shop-template.less";
    @import "../../../styles/common.less";

</style>
<template>
    <Card class="shop-template">
        <div class="shop-main">
            <div class="shop-template-main">
                <div class="shop-template-main-h">
                    <img src="http://imgsrc.baidu.com/imgad/pic/item/64380cd7912397ddc07c52295282b2b7d0a28792.jpg" alt="" style="width:100%;">
                </div>
                <div class="btn">
                    <Button style="display:block;" class="margin-bottom-10" type="primary" @click='start'>启用</Button>
                    <Button style="display:block;" @click="btn">预览</Button>
                </div>
            </div>
        </div>
        <Modal v-model="flag" width='70%;' :styles="{top: '20px'}" title="图片预览">
            <div class="ivu-modals-img">
                <img src="http://imgsrc.baidu.com/imgad/pic/item/64380cd7912397ddc07c52295282b2b7d0a28792.jpg" alt="" style="width:100%;">
            </div>
        </Modal>
        <div class="shop-bottom flexs">
            <div v-for="item,index in data">
                <a href="javascript:;" @click="btnImg(index)">
                    <!-- <img src="http://imgsrc.baidu.com/imgad/pic/item/64380cd7912397ddc07c52295282b2b7d0a28792.jpg" alt="">
                     -->
                     <img :src="item.templateImg" alt="">
                    <p>
                        <span>{{item.templateName}}</span>
                        <span style="font-size:12px;color:#cc9900;">{{item.countTemplate}}</span>
                    </p>
                </a>
                
            </div>

        </div>
    </Card>
</template>

<script>

export default {
    name: 'home',
    components: {

    },
    data () {
        return {
            flag:false,
            data:[],
            newImg:'',//当前的img
            id:'',
        };
    },
    computed: {

    },
    methods: {
        btn(){
            this.flag = true;
        },
        btnImg(index){
            console.log(index);
            this.newImg = this.data[index].templateImg;
            this.id = this.data[index].id;
        },
        start(){ // 点击启用
            this.$http.put('http://192.168.10.108:8080/storeTemplate/updateStoreIsOpen',{
                storeId:"0571ea955c89468f901db24f65de28f9",
                id:this.id
            }).then((data)=>{
                if(data.data.result == 1){
                    this.$Message.info(data.data.message);
                    
                }else{
                    alert(data.data.message)
                }
            }).catch((err)=>{
                console.log(err);
                
            })
        },
        initTem(){ //获取模板
            this.$http.post('http://192.168.10.108:8080/storeTemplate/selectStoreTemplate',{
                storeId:"0571ea955c89468f901db24f65de28f9"
            }).then((data)=>{
                this.data = data.data.list;
                console.log(this.data);
                
            }).catch((err)=>{
                console.log(err);
                
            })
        }
        
    },
    mounted(){
        this.initTem()
    }
};
</script>