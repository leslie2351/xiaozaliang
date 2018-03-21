<!-- 我是收藏统计页面 -->
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
                    title: '订单号',
                    // key: 'orderNo',
                    align: 'center',
                    render: (h, params) => {
                            return h('div', [
                                h('a', {
                                    props: {
                                        href:"javascript:;"
                                    },
                                    style: {
                                        
                                    },
                                    on: {
                                        click: () => {
                                            console.log(params.row.orderNo);
                                            
                                            let argu = {
                                               
                                                order_id: this.id
                                            };
                                            this.$router.push({
                                                name: 'discountInfo',
                                                params: argu
                                            });
                                        }
                                    }
                                }, params.row.orderNo)                           
                            ])
                    }
                },
                {
                    title: '使用时间',
                    key: 'createDate',
                    align: 'center'
                },
                {
                    title: '订单金额',
                    key: 'orderMoney',
                    align: 'center'
                },
                {
                    title: '订单状态',
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
            this.$http.post('http://192.168.5.21:8080/store-coupon-info/pageUseCoupon',{
                coupon_id:this.$route.params.order_id,
                pageNum:this.current,
                pageSize:this.pageSize,
                store_id:6666,
            }).then((data)=>{
                console.log(data.data)
                this.orderData = data.data.list
                this.orderData.forEach((item)=>{
                    item.createDate = item.orderInfoPO.createDate;
                    item.orderMoney = item.orderInfoPO.orderMoney;
                    item.orderNo = item.orderInfoPO.orderNo;
                    if(item.orderInfoPO.orderState == 1){
                        item.onOff = '待付款'
                    }else if(item.orderInfoPO.orderState == 2){
                        item.onOff = '待发货'
                    }else if(item.orderInfoPO.orderState == 3){
                        item.onOff = '待收货'
                    }else if(item.orderInfoPO.orderState == 4){
                        item.onOff = '已完成'
                    }else if(item.orderInfoPO.orderState == 5){
                        item.onOff = '已取消'
                    }else if(item.orderInfoPO.orderState == 6){
                        item.onOff = '售后中'
                    }else if(item.orderInfoPO.orderState == 7){
                        item.onOff = '售后已完成'
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
