
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
                    <h3 class="fl">店铺banner</h3>
                    <Button type="primary" class="fr" @click="btnAdd">新增</Button>
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
                        <span class="fl label-t margin-right-10">名称：</span>
                        <Input v-model="value1" placeholder="..." style="width: 300px"></Input>
         
                    </div>
                    <div class="padding-top-10 display">
                        <span class="fl label-t margin-right-10">图片地址：</span>
                        <Input v-model="value2" placeholder="..." style="width: 300px"></Input>
         
                    </div>
                    <div class="padding-top-10 display">
                        <span class="fl label-t margin-right-10">关联类型：</span>
                        <Select class='fl margin-right-10' v-model="value3" style="width:200px" placeholder='品牌' @on-change='screenOrder'>
                            <Option v-for="item in dictType" :value="item.dictName" :key="item.dictName">{{item.dictName}}</Option>
                        </Select>
                    </div>
                    <div class="padding-top-10 clearfix">
                        <span class="fl label-t margin-right-10">排序：</span>
                        <!-- <Input v-model="value4" placeholder="..." style="width: 300px"></Input>
                         -->
                         <p class='fl' style='font-size:14px;line-height:32px'>{{value4}}</p>
         
                    </div>
                    <div class="padding-top-10">
                        <span class="fl label-t margin-right-10">是否显示：</span>
                        <RadioGroup v-model="value5" @on-change='addT' class='head-l'>
                            <Radio label="是"></Radio>
                            <Radio label="否"></Radio>
                        </RadioGroup>
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
                        <span class="fl label-t margin-right-10">名称：</span>
                        <Input v-model="value1" placeholder="..." style="width: 300px"></Input>
         
                    </div>
                    <div class="padding-top-10 display">
                        <span class="fl label-t margin-right-10">图片地址：</span>
                        <Input v-model="value2" placeholder="..." style="width: 300px"></Input>
         
                    </div>
                    <div class="padding-top-10 display">
                        <span class="fl label-t margin-right-10">关联类型：</span>
                        <Select class='fl margin-right-10' v-model="value3" style="width:200px" placeholder='品牌' @on-change='screenOrder'>
                            <Option v-for="item in dictType" :value="item.dictName" :key="item.dictName">{{item.dictName}}</Option>
                        </Select>
                    </div>
                    <div class="padding-top-10 display">
                        <span class="fl label-t margin-right-10">排序：</span>
                        <Input v-model="value4" placeholder="只能是大于0的整数" style="width: 300px" @on-blur='setValue'></Input>
                    
         
                    </div>
                    <div class="padding-top-10">
                        <span class="fl label-t margin-right-10">是否显示：</span>
                        <RadioGroup v-model="value5" @on-change='addT' class='head-l'>
                            <Radio label="是"></Radio>
                            <Radio label="否"></Radio>
                        </RadioGroup>
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
                    title: '名称',
                    key: 'bannerName',
                    align: 'center'
                },
                {
                    title: '关联类型',
                    key: 'relationTypeName',
                    align: 'center'
                },
                {
                    title: '是否显示',
                    key: 'isOpenName',
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
                                            this.btnDel(params.index)
                                          
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
            value1:'',
            value2:"",
            dictType:[
                {
                    dictName:''
                }
            ],
            dictCode:'',
            value3:'',
            value4:"",
            beforeSort:'',//之前的sort值
            value5:'',
            //点击修改
            modal2:false,
            //点击删除
            modal3:false,
            ids:'',
            valueOne:'',
            smallSort:'',
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

            
        },

        //点击添加时
        btnAdd(){
            this.modal1 = true;
            this.value3 = this.dictType[0].dictName;
            this.dictCode = this.dictType[0].dictCode
            this.value1 = '';
            this.value2 = '';
            this.value4 = '';
            this.value5 = '';
            this.getSort()
        },
        getSort(){
            this.$http.post('http://192.168.10.108:8080/storeBanner/queryMaxSort',{
                storeId:6666,
            })
            .then((data)=>{
                console.log(data);
                this.value4 = data.data;
            }).catch((err)=>{
                console.log(err);
                
            })
        },
        getDictType(){
            this.dictType = [];
            this.$http.post('http://192.168.10.108:8080/systemDict/selectDictType',{
                dictType:"'store_banner_cognate_type','flag'"
            }).then((data)=>{
               
                data.data.forEach((item)=>{
                    if(item.dictType == "store_banner_cognate_type"){
                        this.dictType.push(item)
                    }
                })
                
            }).catch((err)=>{
                console.log(err);
                
            })
        },
        screenOrder(val){ //新增时点击样式选择
            console.log(val);
            this.dictType.forEach((item)=>{
                if(val == item.dictName){
                    this.dictCode = item.dictCode;
                }
            })
        },
        addT(val){ //选择是否展示
            console.log(val);
            
        },
        okAdd(){ //确认添加
            let isOpen = this.value5 == '是' ? 1:2
            this.$http.post('http://192.168.10.108:8080/storeBanner/insertBannerInfo',{
                storeId:6666,
                bannerName:this.value1,
                bannerUrl:this.value2,
                relationType:this.dictCode,
                sort:this.value4,
                isOpen:isOpen
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
            this.$Message.info('取消添加');
        },

        //点击修改时
        btnAttr(index){
            this.modal2 = true;
            this.ids = this.orderData[index].id;
            this.value1 = this.orderData[index].bannerName;
            this.value2 = this.orderData[index].bannerUrl;
            this.value3 = this.orderData[index].relationTypeName;
            this.beforeSort = this.orderData[index].sort;
            this.dictCode = this.orderData[index].relationType
            this.value4 = this.orderData[index].sort;//暂无
            this.value5 = this.orderData[index].isOpenName;
            

        },
        setValue(){ //修改排序时
            console.log(this.value4);
            if(this.value4 <=0){
                this.value4 = 1;
            }
            
        },
        okAtter(){ //确认修改
            let isOpen = this.value5 == '是' ? 1:2
            this.$http.put('http://192.168.10.108:8080/storeBanner/updateBannerInfo',{
                id:this.ids,
                bannerName:this.value1,
                bannerUrl:this.value2,
                relationType:this.dictCode,
                afterSort:this.value4,
                beforeSort:this.beforeSort,
                sort:this.value4,
                isOpen:isOpen
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


        btnDel(val){//点击删除
            console.log(val)
            this.modal3 = true;
            this.ids = this.orderData[val].id;
            this.valueOne = this.orderData[val].storeId;
            this.smallSort = this.orderData[val].sort;
        },

        okDel (){ //点击删除确定时

            this.$http.post('http://192.168.10.108:8080/storeBanner/delete',{
                id:this.ids,
                valueOne:this.valueOne,
                smallSort:this.smallSort,
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
        cancelDel () {
            this.$Message.info('取消删除');
        },
        initTable(){ //初始化列表
            this.$http.post('http://192.168.10.108:8080/storeBanner/selectStoreBanner',{

            }).then((data)=>{
           
                this.orderData = data.data;
            }).catch((err)=>{
                console.log(err);
                
            })
        }
        
    },
    mounted(){
        this.initTable()
        this.getDictType()
    }
};
</script>