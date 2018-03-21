<!-- 赠品页面 -->
<style lang="less">
    @import '../../../styles/common.less';
    .complimentary-t{
        width: 100px;
        text-align:right;
        line-height: 32px;
    }
</style>

<template>
    <div>
        <Row>
            <Col span="24">
                <Card class='clearfix'>

                    <div class="margin-bottom-10 clearfix">
                        <Row class='fl margin-right-10'>
                            <Input v-model="searchConName" @on-click="initTable" icon="search" placeholder="请输入赠品名称搜索..." style="width: 200px" />
                        </Row>
                        <Button type="primary" class="fr" @click="btnAdd">新增</Button>
                    </div>
                    <Row class="margin-top-10 searchable-table-con1" justify="center" align="middle">
                        <Table border :columns="orderColumns" :data="orderData"></Table>
                    </Row>
                    <div class="clearfix">

                        <Page :total=total size="small" class='fr margin-top-20' show-elevator show-sizer show-total
                            @on-change = 'changePage' @on-page-size-change='changePageSize' placement='top'></Page>
                    </div>
                    <Modal
                            v-model="modal3"
                            title= '新增'
                            @on-ok="okAdd"
                            @on-cancel="cancelAdd">
                            <div>
                                <div class="padding-top-10 display">
                                    <span class="fl complimentary-t margin-right-10">赠品名称：</span>
                                    <Input v-model="value1" placeholder="..." style="width: 300px"></Input>
                    
                                </div>
                                <div class="padding-top-10 display">
                                    <span class="fl complimentary-t margin-right-10">赠品价格：</span>
                                    <Input v-model="value2" placeholder="..." style="width: 300px"></Input>
                    
                                </div>
                                <div class="padding-top-10 display">
                                    <span class="fl complimentary-t margin-right-10">赠品可用库存：</span>
                                    <Input v-model="value3" placeholder="..." style="width: 300px"></Input>
                    
                                </div>
                                <div class="padding-top-10 display">
                                    <span class="fl complimentary-t margin-right-10">赠品总库存：</span>
                                    <Input v-model="value4" placeholder="..." style="width: 300px"></Input>
                    
                                </div>
                                <div class="padding-top-10 display">
                                    <span class="fl complimentary-t margin-right-10">赠品图片：</span>
                                    <VueImgInputer v-model="value5" theme="light" size="small" ref="image"
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
                                    <span class="fl complimentary-t margin-right-10">赠品名称：</span>
                                    <Input v-model="value1" placeholder="..." style="width: 300px"></Input>
                    
                                </div>
                                <div class="padding-top-10 display">
                                    <span class="fl complimentary-t margin-right-10">赠品价格：</span>
                                    <Input v-model="value2" placeholder="..." style="width: 300px"></Input>
                    
                                </div>
                                <div class="padding-top-10 display">
                                    <span class="fl complimentary-t margin-right-10">赠品可用库存：</span>
                                    <Input v-model="value3" placeholder="..." style="width: 300px"></Input>
                    
                                </div>
                                <div class="padding-top-10 display">
                                    <span class="fl complimentary-t margin-right-10">赠品总库存：</span>
                                    <Input v-model="value4" placeholder="..." style="width: 300px"></Input>
                    
                                </div>
                                <div class="padding-top-10 display">
                                    <span class="fl complimentary-t margin-right-10">赠品图片：</span>
                                    <VueImgInputer theme="light" size="small" :imgSrc="value5" ref="images"
                                        placeholder='点击修改图片' bottomText='点击重新上传'
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
                    title: '赠品名称',
                    key: 'gift_name',
                    align: 'center'
                },
                {
                    title: '赠品图片',
                    key: 'tinyImg',
                    align: 'center',
                    render: (h, params) => {
                        return h('div',[
                                h('img',{
                                    attrs:{
                                        'src':params.row.gift_img
                                    },
                                    style:{
                                        marginTop:'10px',
                                        width:'50px',
                                        height:'50px'
                                    }
                                })
                            ]
                        )
                    }
                },
                {
                    title: '赠品价格',
                    key: 'gift_price',
                    align: 'center'
                },
                {
                    title: '总库存',
                    key: 'total_stock',
                    align: 'center'
                },
                {
                    title: '可用库存',
                    key: 'available_stock',
                    align: 'center'
                },
                {
                    title: '创建时间',
                    key: 'create_date',
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
                                    on: {
                                        click: () => {
                                            this.btnAttr(params.index)
                                         
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

            //删除
            modal1:false,
            //编辑
            modal2:false,
            //新增：
            modal3:false,
            value1:'',
            value2:'',
            value3:'',
            value4:'',
            value5:'',
            create_date:'',//创建时间
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
        btnAdd(){ //点击新增时
            this.modal3 = true;
            this.value1 = ''
            this.value2 = ''
            this.value3 = ''
            this.value4 = ''
            this.value5 = ''
        },
        okAdd(){
            console.log(this.$refs.image.$el.querySelector('.img-inputer__preview-img'));
            var base64=''
            if(this.$refs.image.$el.querySelector('.img-inputer__preview-img') != null){
                console.log('666');
                
                base64 = this.$refs.image.$el.querySelector('.img-inputer__preview-img').src
                console.log(base64);
            }
            
            this.$http.post('http://192.168.5.21:8080/store-gift-info/add',{
                store_id:6666,
                gift_name:this.value1,
                gift_price:this.value2,
                available_stock:this.value3,
                total_stock:this.value4,
                gift_img:base64
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
        //点击修改时
        btnAttr(index){
            this.modal2 = true;
            this.value1 = this.orderData[index].gift_name;
            this.value2 = this.orderData[index].gift_price;
            this.value3 = this.orderData[index].available_stock;
            this.value4 = this.orderData[index].total_stock;
            this.value5 = this.orderData[index].gift_img;
            console.log(this.value5);
            
            this.create_date = this.orderData[index].create_date;
            this.id = this.orderData[index].id;
        },
        okAtter(){ //确认修改
            const base64 = this.$refs.images.$el.querySelector('.img-inputer__preview-img').src
            this.$http.put('http://192.168.5.21:8080/store-gift-info/edit',{
                store_id:6666,
                id:this.id,
                gift_name:this.value1,
                gift_price:this.value2,
                available_stock:this.value3,
                total_stock:this.value4,
                gift_img:base64,
                create_date:this.create_date,
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
        del(val){  //删除
            this.modal1=true;
            this.id = this.orderData[val].id;
        },

        okDel(){ //点击删除确认的时候
            this.$http.delete('http://192.168.5.21:8080/store-gift-info/delete/'+this.id,{
                
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
        initTable(){//初始化页面
            this.$http.post('http://192.168.5.21:8080/store-gift-info/page',{
                store_id:6666,
                gift_name:this.searchConName,
                pageNum:this.current,
                pageSize:this.pageSize,
            }).then((data)=>{
                console.log(data);
                this.orderData  =data.data.list;
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
