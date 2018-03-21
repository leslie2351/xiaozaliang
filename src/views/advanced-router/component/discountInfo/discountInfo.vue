<style lang="less">
    
    @import '../../../../styles/common.less';
</style>

<template>
        <Card>
            <div style="padding-bottom:10px;border-bottom:2px solid #e9eaec;" class="clearfix">
                    <span @click='reply()' class="fl">
                        <Icon type="reply" style='font-size:20px;'></Icon>
                    </span>
                    <h3 class="fl margin-left-10">优惠券详情</h3>
                    
                </div>
            <!-- <Tabs :animated='false'> -->
            <Tabs @on-click='btn'>
                <TabPane label="使用情况" icon="social-usd"><tab1 v-if='name == 0'></tab1></TabPane>
                <TabPane label="领用情况" icon="android-map"><tab2 v-if='name == 1'></tab2></TabPane>
                
            </Tabs>
        </Card>
</template>

<script>
import tab1 from './component/tab1'
import tab2 from './component/tab2'

export default {
    name: 'mutative-router',
    components:{
        tab1,
        tab2
    },
    data () {
        return {
            name:0
        }
    },
    computed: {

    },
    methods:{
        btn(name){
            this.name = name;

        },
        reply(){
            this.$router.push({
                name: 'discount',
                
            });
        }
    },
    mounted () {
      

    }
};
</script>
