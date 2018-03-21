<style lang="less">
    // @import "./commodity-list.less";
    @import "../../../styles/common.less";
</style>

<template>
        <Card>
            <Tabs :animated='false' @on-click='btn'>
                <TabPane label="满减满赠" icon="social-usd"><tab1 v-if='name == 0'></tab1></TabPane>
                <TabPane label="单品立减" icon="android-map"><tab2 v-if='name == 1'></tab2></TabPane>
                <TabPane label="满减打折" icon="android-map"><tab3 v-if='name == 2'></tab3></TabPane>
                <TabPane label="满免" icon="android-map"><tab4 v-if='name == 3'></tab4></TabPane>
          
            </Tabs>
        </Card>
</template>

<script>
import tab1 from './components/tab1'
import tab2 from './components/tab2'
import tab3 from './components/tab3'
import tab4 from './components/tab4'

export default {
    name: 'mutative-router',
    components:{
        tab1,
        tab2,
        tab3,
        tab4,
      
    },
    data () {
        return {
            name:0,
        }
    },
    computed: {

    },
    methods:{
        btn(name){
            this.name = name;
            
        }
    },
    mounted () {
        console.log('进入了退款退货单')

    }
};
</script>
