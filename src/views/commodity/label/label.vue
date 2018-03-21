
<style lang="less">
    // @import "./grouping-manage.less";
    @import "../../../styles/common.less";
    .head-l{
            padding-top:6px;
        }

        .label-t{
            width: 100px;
            text-align:right;
            line-height: 32px;
        }
</style>
<template>
        <Row :gutter="10">
            <Card style="width:99.5%; margin:0 auto;">
                <div style="padding-bottom:10px;border-bottom:2px solid #e9eaec;" class="clearfix">
                    <h3 class="fl">标签管理</h3>
                    <Button type="primary" class="fr" @click="btnAdd">添加标签</Button>
                </div>
                <Row class="margin-top-10 searchable-table-con1" justify="center" align="middle">
                    <Table border :columns="orderColumns" :data="orderData"></Table>
                </Row>
                <div class="clearfix">
                    <Page :total=total size="small" class='fr margin-top-20' show-elevator show-sizer show-total
                        @on-change = 'changePage' @on-page-size-change='changePageSize' placement='top'></Page>
                </div>
            </Card>
            <Modal
                v-model="modal1"
                title= '添加'
                @on-ok="okAdd"
                @on-cancel="cancelAdd">
                <div>
                    <div class="padding-top-10 display">
                        <span class="fl label-t margin-right-10">标签名称：</span>
                        <Input v-model="value1" placeholder="..." style="width: 300px" @on-blur='cha'></Input>
                        <p v-if='flag' style="padding-left:110px;color:red;">标签名不能重复</p>
                    </div>
                    <div class="padding-top-10 display">
                        <span class="fl label-t margin-right-10">排序：</span>
                        
                         <p class='fl' style='font-size:14px;line-height:32px'>{{value4}}</p>
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
                        <span class="fl label-t margin-right-10">标签名称：</span>
                        <Input v-model="value1" placeholder="..." style="width: 300px"></Input>
                        
                    </div>
                    <div class="padding-top-10 display">
                        <span class="fl label-t margin-right-10">排序：</span>
                        <Input v-model="value4" placeholder='只能是大于0的整数' style="width: 300px" @on-blur='setValue'></Input>
                    </div>
                </div>

            </Modal>
            <Modal
                v-model="modal3"
                title="确认删除"
                @on-ok="okDel"
                @on-cancel="cancelDel">
                <p>确定删除吗？删除后无法恢复</p>

            </Modal>
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
                    title: '标签名称',
                    key: 'labelName',
                    align: 'center'
                },
                {
                    title: '商品个数',
                    key: 'goodsNum',
                    align: 'center'
                },
                {
                    title: '创建时间',
                    key: 'createDate',
                    align: 'center'
                },
                {
                    title: '排序',
                    key: 'sort',
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
                                        
                                        margin:'5px auto'
                                    },
                                    on: {
                                        click: () => {
                                            this.btnAttr(params.index)
                                          
                                        }
                                    }
                                }, '修改'),
                                h('Button', {
                                    props: {
                                        type: 'success',
                                        size: 'small'
                                    },
                                    style: {
                                       marginLeft:"10px",
                                    },
                                    on: {
                                        click: () => {
                                            this.show(params.index)
                                            let argu = {
                                                // btn_id:this.id,
                                                order_id: this.id
                                            };
                                            this.$router.push({
                                                name: 'commodity-set',
                                                params: argu
                                            });
                                        }
                                    }
                                }, '商品设置'),
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
            //点击添加
            modal1:false,
            flag:false,
            value1:'',
            value4:'',
            //点击修改
            modal2:false,
            ids:'',
            sortOld:'',//修改前的排序
            //删除
            modal3:false,
            sortlist:[],
            total:0,//总页数
            current:1,//当前页码
            pageSize:10,//每页数量
        };
    },
    computed: {

    },
    methods: {
        changePage(val){ //切换页码
            console.log(val)
            this.current = val
            this.initTable()
        },
        changePageSize(val){ //改变每页显示条数
            this.pageSize = val
            this.initTable()

        },
        show(val){
            this.id = this.orderData[val].id;
            console.log(this.id);
            
        },
        cha(){ // 添加时失去焦点验证标签名

            this.$http.post('http://192.168.5.11:8080/label/checkLabelName',{
                    storeId:6666,
                    labelName:this.value1,

                }).then((data)=>{
               
                    if(data.data.result == "0"){
                        
                        this.flag = true;
                    }else if(data.data.result == "1"){
    
                        this.flag = false;
                    }
                    
                }).catch((err)=>{
                    console.log(err)

                })
        },
        //点击添加时
        btnAdd(){
            this.modal1 = true;
            this.value1 = '';
            this.value4 = '';
            this.getMax()
        },
        getMax(){ //获取最大值
            this.$http.get('http://192.168.5.11:8080/label/maxSort/6666'
            ).then((data)=>{
                console.log(data);
                this.value4 = data.data;

                }).catch((err)=>{
                    console.log(err)

                })
        },
        okAdd(){ //确认添加

            if(!this.flag){
                this.$http.post('http://192.168.5.11:8080/label/addlabel',{
                    storeId:6666,
                    labelName:this.value1,
                    sort:this.value4

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
            }else{
                alert('标签名重复，添加失败')
            }
        },
        cancelAdd(){
            this.$Message.info('取消添加');
        },
        //点击删除
        del(val){
            this.modal3 = true;
            this.id = "'"+this.orderData[val].id +"'";
            this.sortlist=[]
            this.sortlist.push(this.orderData[val].sort)

        },
        okDel(){ //点击删除确认的时候
            this.$http.post('http://192.168.5.11:8080/label/delLabel',{
                id:this.id,
                valueOne:"6666", //当前这个是店铺id
                sort:this.sortlist,
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
        //点击修改时
        btnAttr(index){
            this.modal2 = true;
            this.ids = this.orderData[index].id;
            this.value1 = this.orderData[index].labelName;
            this.value4 = this.orderData[index].sort;
            this.sortOld= this.orderData[index].sort;
        },
        setValue(){ //修改排序时
            console.log(this.value4);
            if(this.value4 <=0){
                this.value4 = 1;
            }
            
        },
        okAtter(){ //确认修改

            this.$http.put('http://192.168.5.11:8080/label/alterLabel',{
                storeId:6666,
                id:this.ids,
                labelName:this.value1,
                sort:this.value4,
                sortOld:this.sortOld,
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

        initTable(){ //初始化列表
            this.$http.post('http://192.168.5.11:8080/label/labelShow',{
                storeId:6666,
                pageNum:this.current,
                pageSize:this.pageSize,
            }).then((data)=>{
               
                this.orderData = data.data.list;
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