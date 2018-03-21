<!-- 优惠券页面 -->
<style lang="less">

    @import '../../../styles/common.less';
    .discount{
        .ivu-table-cell{
            padding: 0;
        }
        
    }
    .discount-t{
        width: 100px;
        text-align:right;
        line-height: 32px;
    }
</style>

<template>
    <div class="discount">
        <Row>
            <Col span="24">
                <Card class='clearfix'>

                    <div class="margin-bottom-10 clearfix">
                        <Row class='fl margin-right-10'>
                            <Input v-model="searchConName" @on-click="initTable" icon="search" placeholder="请输入商品名称搜索..." style="width: 200px" />
                        </Row>
                        <div class='fl padding-left-20'>
                            <span class="fl" style="color: #495060;font-size: 12px;line-height: 32px;">生效时间：</span>
                            <DatePicker type="datetime" :value='startDate' @on-change='changeDate1' placeholder="生效时间" style="width: 200px"></DatePicker>
                        </div>
                        <div class='fl padding-left-20'>
                            <span class="fl" style="color: #495060;font-size: 12px;line-height: 32px;">失效时间：</span>
                            <DatePicker type="datetime" :value='endDate' @on-change='changeDate2' placeholder="失效时间" style="width: 200px"></DatePicker>
                        </div>
                        <Button type="primary" class="fr" @click="btnAdd">新增</Button>
                    </div>
                    <Row class="margin-top-10 searchable-table-con1" justify="center" align="middle">
                        <Table border :columns="orderColumns" :data="orderData"></Table>
                    </Row>
                    <div class="clearfix margin-top-20">
                        <Page :total=total size="small" class='fr' show-elevator show-sizer show-total
                            @on-change = 'changePage' @on-page-size-change='changePageSize' placement='top'></Page>
                    </div>
                    <Modal
                            v-model="modal3"
                            title= '新增'
                            @on-ok="okAdd"
                            @on-cancel="cancelAdd">
                            <div>
                                <div class="padding-top-10 display">
                                    <span class="fl discount-t margin-right-10">优惠券名称：</span>
                                    <Input v-model="value1" placeholder="..." style="width: 300px"></Input>
                    
                                </div>
                                <div class="padding-top-10 display">
                                    <span class="fl discount-t margin-right-10">优惠门槛：</span>
                                    <Input v-model="value2" placeholder="..." style="width: 300px"></Input>
                    
                                </div>
                                <div class="padding-top-10 display">
                                    <span class="fl discount-t margin-right-10">优惠金额：</span>
                                    <Input v-model="value3" placeholder="..." style="width: 300px"></Input>
                    
                                </div>
                                <div class="padding-top-10 display">
                                    <span class="fl discount-t margin-right-10">生效时间：</span>
                                    <DatePicker type="datetime" :value='value4' @on-change='changeDateT' placeholder="生效时间" style="width: 200px"></DatePicker>
                    
                                </div>
                                <div class="padding-top-10 display">
                                    <span class="fl discount-t margin-right-10">失效时间：</span>
                                    <DatePicker type="datetime" :value='value5' @on-change='changeDateS' placeholder="生效时间" style="width: 200px"></DatePicker>
                    
                                </div>
                                <div class="padding-top-10 display">
                                    <span class="fl discount-t margin-right-10">优惠券数量：</span>
                                    <Input v-model="value6" placeholder="..." style="width: 300px"></Input>
                    
                                </div>
                       
                                <div class="padding-top-10 display">
                                    <span class="fl discount-t margin-right-10">优惠券图标：</span>
                                    <VueImgInputer v-model="value7" theme="light" size="small" ref="image"
                                        placeholder='点击上传图片' bottomText='点击重新上传'
                                    ></VueImgInputer>
                    
                                </div>
                            </div>
                    </Modal>
                    <Modal
                            v-model="modal2"
                            title= '修改'
                            @on-ok="okAtter"
                            @on-cancel="cancelAtter">
                            <div>
                                <div class="padding-top-10 display">
                                    <span class="fl discount-t margin-right-10">优惠券名称：</span>
                                    <Input v-model="value1" placeholder="..." style="width: 300px"></Input>
                    
                                </div>
                                <div class="padding-top-10 display">
                                    <span class="fl discount-t margin-right-10">优惠门槛：</span>
                                    <Input v-model="value2" placeholder="..." style="width: 300px"></Input>
                    
                                </div>
                                <div class="padding-top-10 display">
                                    <span class="fl discount-t margin-right-10">优惠金额：</span>
                                    <Input v-model="value3" placeholder="..." style="width: 300px"></Input>
                    
                                </div>
                                <div class="padding-top-10 display">
                                    <span class="fl discount-t margin-right-10">生效时间：</span>
                                    <DatePicker type="datetime" :value='value4' @on-change='changeDateT' placeholder="生效时间" style="width: 200px"></DatePicker>
                    
                                </div>
                                <div class="padding-top-10 display">
                                    <span class="fl discount-t margin-right-10">失效时间：</span>
                                    <DatePicker type="datetime" :value='value5' @on-change='changeDateS' placeholder="生效时间" style="width: 200px"></DatePicker>
                    
                                </div>
                                <div class="padding-top-10 display">
                                    <span class="fl discount-t margin-right-10">优惠券数量：</span>
                                    <Input v-model="value6" placeholder="..." style="width: 300px"></Input>
                    
                                </div>
                       
                                <div class="padding-top-10 display">
                                    <span class="fl discount-t margin-right-10">优惠券图标：</span>
                                    <VueImgInputer v-model="value7" theme="light" size="small" ref="images"
                                        placeholder='点击上传图片' bottomText='点击重新上传'
                                    ></VueImgInputer>
                    
                                </div>
                            </div>
                    </Modal>
                    <Modal
                        v-model="modal1"
                        title="确认删除"
                        @on-ok="okDel"
                        @on-cancel="cancelDel">
                        <p>确定删除吗？删除后无法恢复</p>

                    </Modal>

                </Card>
            </Col>
        </Row>
    </div>
</template>

<script>
import VueImgInputer from 'vue-img-inputer'
export default {
    name: 'mutative-router',
    components:{
            VueImgInputer,
    },
    data () {
        return {
            orderColumns: [

                {
                    title: '优惠券名称',
                    key: 'couponName',
                    align: 'center'
                },
                {
                    title: '优惠门槛',
                    key: 'fullMoney',
                    align: 'center'
                },
                {
                    title: '优惠金额',
                    key: 'reductionMoney',
                    align: 'center'
                },
                {
                    title: '优惠券图标',
                    key: 'couponImg',
                    align: 'center'
                },
                {
                    title: '生效时间',
                    key: 'startDate',
                    align: 'center'
                },
                {
                    title: '失效时间',
                    key: 'endDate',
                    align: 'center'
                },
                {
                    title: '优惠券数量',
                    key: 'couponNum',
                    align: 'center'
                },
                {
                    title: '操作',
                    key: 'operation',
                    align: 'center',
                    render: (h, params) => {
                            return h('div', [
                                h('Button', {
                                    props: {
                                        type: 'primary',
                                        size: 'small'
                                    },
                                    style: {
                                        
                                    },
                                    on: {
                                        click: () => {
                                            this.show(params.index)
                                            let argu = {
                                               
                                                order_id: this.id
                                            };
                                            this.$router.push({
                                                name: 'discountInfo',
                                                params: argu
                                            });
                                        }
                                    }
                                }, '查看'),
                                h('Button', {
                                    props: {
                                        type: 'success',
                                        size: 'small'
                                    },
                                    style: {
                                        marginLeft:"10px"
                                    },
                                    on: {
                                        click: () => {
                                            this.btnAtter(params.index)
                                        }
                                    }
                                }, '修改'),
                                h('Button', {
                                    props: {
                                        type: 'error',
                                        size: 'small'
                                    },
                                    style: {
                                        marginLeft:"10px"
                                    },
                                    on: {
                                        click: () => {
                                            this.del(params.index)
                                        }
                                    }
                                }, '删除'),                              
                            ])
                    }
                }

            ],
            orderData: [],
            searchConName:'',

            id:'',//
            
            startDate:'',
            endDate:"",
            //新增
            modal3:false,
            value1:"",
            value2:"",
            value3:"",
            value4:"",
            value5:"",
            value6:"",
            value7:"",
            value8:'',
            //删除
            modal1:false,
            //修改
            modal2:false,

            createDate:'',
            groupId:'',//分组id
            total:0,//总页数
            current:1,//当前页码
            pageSize:10,//每页数量
            
        };
    },

    methods:{
        //顶部筛选
        changeDate1(val){
            this.startDate = val
            this.initTable()
        },
        changeDate2(val){
            this.endDate = val;
            this.initTable()
        },
        //新增修改
        changeDateT(val){
            console.log(val);
            
            this.value4 = val
        },
        changeDateS(val){
            this.value5 = val
        },
        changePage(val){ //切换页码
            console.log(val)
            this.current = val
            this.initTable()
        },
        changePageSize(val){ //改变每页显示条数
            this.pageSize = val
            this.initTable()

        },
        btnAdd(){ //点击新增时
            this.modal3 = true;
        },
        okAdd(){
            const base64 = this.$refs.image.$el.querySelector('.img-inputer__preview-img').src
            this.$http.post('http://192.168.5.21:8080/store-coupon-info/add',{
                store_id:6666,
                couponType:1,
                couponName:this.value1,
                fullMoney:this.value2,
                reductionMoney:this.value3,
                startDate:this.value4,
                endDate:this.value5,
                couponNum:this.value6,
                couponImg:base64,

            }).then((data)=>{
                console.log(data);
                if(data.data.result == 1){
                    this.$Message.info(data.data.message);
                    this.initTable()
                }else{
                    alert(data.data.message)
                }
            }).catch((err)=>{
                console.log(err);
                
            })
        },
        cancelAdd(){

        },
        show(val){  //查看
            this.id = this.orderData[val].id;
            console.log(this.id);
            
        },
        del(val){  //删除
            this.modal1=true;
            this.id = this.orderData[val].id;
        },
        okDel(){ //点击删除确认的时候
            this.$http.delete('http://192.168.5.21:8080/store-coupon-info/delete/'+this.id,{
            
            }).then((data)=>{
                console.log(data)
                if(data.data.result == 1){
                    this.$Message.info(data.data.message);
                    this.initTable()
                }else{
                    alert(data.data.message)
                }
            }).catch((err)=>{
                console.log(err)

            })
        },
        cancelDel(){
            this.$Message.info('取消删除');
        },
        btnAtter(val){  //修改
            this.modal2 = true;
            this.id = this.orderData[val].id;
            this.createDate = this.orderData[val].createDate;
            this.value1 = this.orderData[val].couponName;
            this.value2 = this.orderData[val].fullMoney;
            this.value3 = this.orderData[val].reductionMoney;
            this.value4 = this.orderData[val].startDate;
            this.value5 = this.orderData[val].endDate;
            this.value6 = this.orderData[val].couponNum;
            this.value7 = this.orderData[val].couponImg;
        },
        okAtter(){ //确认修改
            console.log(this.value7);
            const base64 = this.$refs.images.$el.querySelector('.img-inputer__preview-img').src
            this.$http.put('http://192.168.5.21:8080/store-coupon-info/edit',{
                store_id:6666,
                id:this.id,
                couponType:1,
                couponName:this.value1,
                fullMoney:this.value2,
                reductionMoney:this.value3,
                couponImg:base64,
                startDate:this.value4,
                endDate:this.value5,
                couponNum:this.value6,
                createDate:this.createDate

            }).then((data)=>{
                if(data.data.result == 1){
                    this.$Message.info(data.data.message);
                    this.initTable()
                }else{
                    alert(data.data.message)
                }
            }).catch((err)=>{
                console.log(err);
                
            })
         
        },
        cancelAtter(){
            this.$Message.info('取消修改');
        },
        initTable(){//初始化页面
            this.$http.post('http://192.168.5.21:8080/store-coupon-info/page',{
                store_id:6666,
                couponName:this.searchConName,
                startDate:this.startDate,
                endDate:this.endDate,
                pageNum:this.current,
                pageSize:this.pageSize,
            }).then((data)=>{
                console.log(data);
                this.orderData = data.data.list
                this.total = data.data.total
            }).catch((err)=>{
                console.log(err);
                
            })
        },

    },
    mounted(){
        this.initTable()


    }
};
</script>
