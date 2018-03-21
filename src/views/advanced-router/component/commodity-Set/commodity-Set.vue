// 商品设置
<style lang="less">
    // @import "./grouping-manage.less";
    @import "../../../../styles/common.less";
            .set-t{
            width: 100px;
            text-align:right;
            line-height: 32px;
        }
</style>
<template>
        <Row :gutter="10">
            <Card style="width:99.5%; margin:0 auto;">
                <div style="padding-bottom:10px;border-bottom:2px solid #e9eaec;" class="clearfix">
                    <span @click='reply()' class="fl">
                        <Icon type="reply" style='font-size:20px;'></Icon>
                    </span>
                    <h3 class="fl margin-left-10">商品设置</h3>
                    <Button type="primary" class="fr" @click="addPath">添加商品</Button>
                </div>
                <Row class="margin-top-10 searchable-table-con1" justify="center" align="middle">
                    <Table border :columns="orderColumns" :data="orderData" @on-selection-change='change'></Table>
                </Row>
                <div class="clearfix">
                    <Button type="error" class="fl margin-top-10" @click="bathDel">批量删除</Button>
                    <Page :total=total size="small" class='fr margin-top-20' show-elevator show-sizer show-total
                        @on-change = 'changePage' @on-page-size-change='changePageSize' placement='top'></Page>
                </div>
                <Modal
                    v-model="modal1"
                    title="确认删除"
                    @on-ok="okDel"
                    @on-cancel="cancelDel">
                    <p>确定删除吗？删除后无法恢复</p>

                </Modal>
            <Modal
                v-model="modal2"
                title= '修改'
                @on-ok="okAtter"
                @on-cancel="cancelAtter">
                <div>
        
                    <div class="padding-top-10 display">
                        <span class="fl set-t margin-right-10">排序：</span>
                        <Input v-model="value1" placeholder="..." style="width: 300px"></Input>
                    </div>
                </div>

            </Modal>
            </Card>
            
        </Row>
</template>

<script>

export default {
    name: 'home',
    components: {

    },
    data () {
        return {
            orderColumns: [
                {
                    type: 'selection',
                    width: 60,
                    align: 'center'
                },
                {
                    title: '商品名称',
                    key: 'tinyImg',
                    align: 'center',
                    render: (h, params) => {
                        return h('div',[
                                h('img',{
                                    attrs:{
                                        'src':params.row.tinyImg
                                    },
                                    style:{
                                        marginTop:'10px',
                                        width:'50px',
                                        height:'50px'
                                    }
                                }),
                                h('div',{
                                    style:{
                                        // width:'50px',
                                        // height:'50px',
                                        margin:'0 auto'
                                    }
                                },params.row.goodsName)
                            ]
                        )
                    }
                },
                {
                    title: '商品价格',
                    key: 'salePrice',
                    align: 'center'
                },
                {
                    title: '库存',
                    key: 'totalStock',
                    align: 'center'
                },
                {
                    title: '可用库存',
                    key: 'availableStock',
                    align: 'center'
                },
                {
                    title: '排序值',
                    key: 'sort',
                    align: 'center'
                },
                {
                    title: '上架时间',
                    key: 'shelvesDate',
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
            orderData: [
                
            ],

            //删除
            modal1:false,
            //修改
            modal2:false,
            value1:'',
            sortOld:'',
            sortList:[],
            id:'',
            changselect:[],//批量选中的列表
            changselectId:[],//批量选中的列表的ID
            total:0,//总页数
            current:1,//当前页码
            pageSize:10,//每页数量
        };
    },
    computed: {

    },
    methods: {
        change(selection){//当批量选中的状态发生改变时
            this.changselectId=[]; //清空之前选择的ID数组
            this.sortList = [];
            this.changselect=selection;
            console.log(this.changselect);
            
            this.changselect.forEach((item)=>{
                this.changselectId.push(item.sid)
                this.sortList.push(item.sort)
            })
            let ss = ''
            this.changselectId.forEach((item)=>{
                ss+="'"+item + "'"+","
                // ss+=item+","
            })
            this.id = ss.substr(0,ss.length-1);
            console.log(this.id);
            console.log(this.sortList);
            
            
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
        reply(){
            this.$router.push({
                name: 'label',
                
            });
        },
        //点击删除
        del(val){
            this.modal1 = true;
            this.id = "'"+this.orderData[val].sid +"'";
            this.sortList = [];
            this.sortList.push(this.orderData[val].sort)

        },
        //点击批量删除
        bathDel(){
            if(this.id.length<=0){
                alert('请选择商品')
            }else{
                this.modal1 = true;
            }
        },
        okDel(){ //点击删除确认的时候
            this.$http.post('http://192.168.5.11:8080/labelGoodsMiddle/delassociatedGoods',{
                id:this.id,
                valueOne:this.$route.params.order_id,
                sort:this.sortList,
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
        //点击修改
        btnAtter(index){
            console.log(this.orderData);
            
            this.modal2 = true;
            this.id = this.orderData[index].sid;
            this.value1 = this.orderData[index].sort;
            this.sortOld = this.orderData[index].sort;

        },
        okAtter(){ //确认修改

            this.$http.put('http://192.168.5.11:8080/labelGoodsMiddle/alterGoodsSort',{
                // storeId:6666,
                id:this.id,
                sort:this.value1,
                sortOld:this.sortOld,
                labelId:this.$route.params.order_id,
            }).then((data)=>{
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
        cancelAtter(){
            this.$Message.info('取消修改');
        },
        //点击添加商品
        addPath(){
            let argu = {              
                order_id: this.$route.params.order_id
            };
            this.$router.push({
                name: 'commodity-add',
                params: argu
            });
        },
        initTable(){
            this.$http.post('http://192.168.5.11:8080/label/goodsSetInfo',{
                id:this.$route.params.order_id,
                pageNum:this.current,
                pageSize:this.pageSize,
            }).then((data)=>{
                console.log(data);
                this.orderData = data.data.list
                this.total = data.data.total
            }).catch((err)=>{
                console.log(err);
                
            })
        }
        
    },
    mounted(){
        this.initTable()
    }
};
</script>