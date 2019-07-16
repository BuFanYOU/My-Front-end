<template>
    <div>
         <Input size="large" style= "background-color:transparent;border:0;" />
        <Table border ref="selection" :columns="columns41" :data="addLists"></Table> 
        <!-- <div class="go-to">
          <Button type="primary" style="margin-right:10px">解除封禁</Button>
          <Button type="error" >封禁店铺</Button>
        </div> -->
    </div>
</template>

<script>
import store from '@/vuex/store';
import { mapState, mapActions } from 'vuex';
import Distpicker from 'v-distpicker';
    export default {
        name: 'AllStore',
        data() {
            return {
                flag: false,
                addLists:[
                    // {
                    // store_id: '00123',
                    // store_name: '李宁',
                    // store_username: 'Mark',
                    // store_phone_num: '2492310',
                    // store_position: '北京'
                    // }
                ],
                columns41: [
                    {
                    title: '店铺ID',
                    width: 150,
                    key: 'storeId'
                },
                  {
                    title: '店铺名称',
                    width: 150,
                    key: 'name'
                },
                {
                    title: '店铺负责人',
                    width: 150,
                    key: 'managerId'
                },
                {
                    title: '店铺固定电话',
                    key: 'phone',
                    align : 'center',
                    className: "demo",
                    render: (h, params) => {
                        return h('div', [
                            h('Input', {
                                props: {
                                    value: this.addLists[params.index].phone,
                                    size: 'small',
                                },
                                style: {
                                    marginRight: '5px',
                                    display: this.flag ? '' : 'none'
                                },
                                on: {
                                    'on-blur': (event) => {
                                        this.addLists[params.index].phone = event.target.value
                                    }
                                },
                            }, ''), 
                            h('span',{
                                style: {
                                    marginRight: '5px',
                                    display: this.flag ? 'none' : ''
                                },
                            },this.addLists[params.index].phone),
                        ]);
                    }
                },
                {
                    title: '店铺所在地',
                    key: 'position',
                    align : 'center',
                    className: "demo",
                    render: (h, params) => {
                        return h('div', [
                            h('Input', {
                                props: {
                                    value: this.addLists[params.index].position,
                                    size: 'small',
                                },
                                style: {
                                    marginRight: '5px',
                                    display: this.flag ? '' : 'none'
                                },
                                on: {
                                    'on-blur': (event) => {
                                        this.addLists[params.index].position = event.target.value
                                    }
                                },
                            }, ''), 
                            h('span',{
                                style: {
                                    marginRight: '5px',
                                    display: this.flag ? 'none' : ''
                                },
                            },this.addLists[params.index].position),
                        ]);
                    }
                },
                {
                    title: '操作',
                    key: 'action',
                    width: 250,
                    align: 'center',
                    render: (h, params) => {
                        return h('div', [
                            h('Button', {
                                props: {
                                    type: 'primary',
                                    size: 'small'
                                },
                                style: {
                                    marginRight: '20px',
                                    display: this.flag ? 'none' : ''
                                },
                                on: {
                                    click: () => {
                                        this.edit(params.index)
                                    }
                                }
                            }, '修改'),
                            h('Button', {
                                props: {
                                    type: 'primary',
                                    size: 'small'
                                },
                                style: {
                                    marginRight: '20px',
                                    display: this.flag ? '' : 'none'
                                },
                                on: {
                                    click: () => {
                                        this.save(params.index)
                                    }
                                }
                            }, '保存'),
                            h('Button', {
                                props: {
                                    type: 'error',
                                    size: 'small'
                                },
                                on: {
                                    click: () => {
                                        this.remove(params.index)
                                    }
                                }
                            }, '删除')
                        ])
                    }
                }
                ],
                
            }
        },
    created () {
      this.$axios.get('http://127.0.0.1:8080/store/list')
      .then(success => {
        this.addLists = success.data;
      })
      .catch(error => {
        console.log(error);
      })
     },
    computed: {
    ...mapState(['userInfo'])
    },
    methods: {
         edit(index) {
            this.flag = !this.flag;
         },
        save(index) {
            this.flag = !this.flag;
            this.$axios.put('http://127.0.0.1:8080/store',{
              storeId: this.addLists[index].storeId,
              name: this.addLists[index].name,
              managerId: this.addLists[index].managerId,
              phone: this.addLists[index].phone,
              position: this.addLists[index].position
            })
            .then(success => {
              this.$Message.success('修改成功');
            })
            .catch(error => {
              this.$Message.error('修改失败');
            })
        },
        remove(index){
            this.addLists.splice(index, 1);
            this.$axios.delete('http://127.0.0.1:8080/store',{
              params:{storeId: this.addLists[index].storeId}
            })
            .then(success => {
              this.$Message.success('删除成功');
            })
            .catch(error => {
              this.$Message.error('删除失败');
            })
        }
     },
     store
    }
</script>

<style>
    /* .demo{
        color: #FF0000;
    } */
    .go-to {
        margin: 15px;
        display: flex;
        justify-content: flex-end;
    }

</style>