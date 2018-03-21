<!-- 领取情况 -->
<style lang="less">

    @import '../../../../../styles/common.less';
    .ivu-table-cell{
        padding:0;
    }
</style>

<template>
    <div>
        <Row>
            <Col span="24">
                <Card class='clearfix'>

                    <Row class="margin-top-10 searchable-table-con1" justify="center" align="middle">
                        <Table border :columns="orderColumns" :data="orderData"></Table>
                    </Row>
                    <div class="clearfix">
                        <Page :total=total size="small" class='fr margin-top-20' show-elevator show-sizer show-total
                            @on-change = 'changePage' @on-page-size-change='changePageSize' placement='top'></Page>
                    </div>
                </Card>
            </Col>
        </Row>
    </div>
</template>

<script>
export default {
    name: 'mutative-router',
    data () {
        return {

            orderColumns: [
                {
                    title: '用户名',
                    key: 'userName',
                    align: 'center'
                },
                {
                    title: '领取时间',
                    key: 'receiveDate',
                    align: 'center'
                },
                {
                    title: '状态',
                    key: 'onOff',
                    align: 'center'
                },

            ],
            orderData: [

            ],

            total:0,//总页数
            current:1,//当前页码
            pageSize:10,//每页数量
        };
    },

    methods:{
        changePage(val){ //切换页码
            console.log(val)
            this.current = val
            this.initTable()
        },
        changePageSize(val){ //改变每页显示条数
            this.pageSize = val
            this.initTable()

        },
        initTable(){//初始化页面
            this.$http.post('http://192.168.5.21:8080/store-coupon-info/pageUserCoupon',{
                store_id:6666,
                couponId:this.$route.params.order_id,
                pageNum:this.current,
                pageSize:this.pageSize,
                
            }).then((data)=>{
                console.log(data.data)
                this.orderData = data.data.list
                this.orderData.forEach((item)=>{
                    if(item.flag == 1){
                        item.onOff = '已使用'
                    }else if(item.flag == 2){
                        item.onOff = '未使用'
                    }else if(item.flag == 3){
                        item.onOff = '过期'
                    }
                })
                this.total = data.data.total
            }).catch((err)=>{
                console.log(err)

            })
        },

    },
    mounted(){
        this.initTable()
    }
};
</script>
