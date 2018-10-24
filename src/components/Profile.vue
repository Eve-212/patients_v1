<template>
    <div :class="{expanded: isExpanded}">
        <div class="card-wrap">
            <div>柯博文｜<span>Obstetrics and gynaecology units</span></div>
            <div>
                <div class="btn" :class="{followed:items.basic.followBtn.clicked}" @click="follow()">
                    <i class="fa fa-star"></i>{{ items.basic.followBtn.text }}
                </div>
                <a class="btn" href="mailto:dumbo750212@msn.com"><i class="fa fa-envelope"></i>Email</a>
            </div>
        </div>
        <div class="content-wrap">
            <router-view 
                :lists="items.basic.showData"
                :badgeCount="items.basic.badgeCount"  
                @unfollow="unfollow" 
                @selectedType="selectedType">
            </router-view>
        </div>
    </div>
</template>

<script>
    import Vue from 'vue'
    import APIClient from 'moleculer-openapi-client'

    Vue.component('table-component', require('@/components/Table.vue').default)

    export default {

      props:['isExpanded'],

      beforeRouteUpdate(to, from, next) {
        next();
      }, 
      data() {
        return {
            items:{
                basic:{
                    isExpanded: false,
                    badgeCount:[],
                    followBtn: {
                        text: 'Follow',
                        clicked: false
                    },
                    showData: {
                        tableTitle:[],
                        tableContent:[]
                    },
                },
                lists:{
                    incompeleted:{
                        tableTitle:['Order', 'Id', 'Name', 'Hospitalization Date'],
                        tableContent:[
                            {
                                id: 12,
                                name: 'Jessica',
                                date: '2018-10-22', 
                                compeleted: false
                            },
                            {
                                id: 13,
                                name: 'Joan',
                                date: '2018-10-01',
                                compeleted: false
                            },
                            {
                                id: 14,
                                name: 'Debby',
                                date: '2018-09-22',
                                compeleted: false
                            }
                        ]
                    },
                    compeleted:{
                        tableTitle : ['Order', 'Id', 'Name', 'Compeleted On'],
                        tableContent:[
                            {
                                id: 122354132451345,
                                name: 'Jessica',
                                date: '2018-10-22',
                                compeleted: true   
                            },
                            {
                                id: 12,
                                name: 'Jessica',
                                date: '2018-10-22',
                                compeleted: true   
                            },
                            {
                                id: 12,
                                name: 'Jessica',
                                date: '2018-10-22',
                                compeleted: true   
                            },
                            {
                                id: 13,
                                name: 'Joan',
                                date: '2018-10-01',
                                compeleted: true
                            },
                            {
                                id: 14,
                                name: 'Wang',
                                date: '2018-09-22',
                                compeleted: true
                            },
                            {
                                id: 15,
                                name: 'Kitty',
                                date: '2018-10-01',
                                compeleted: true
                            },
                            {
                                id: 12,
                                name: 'Jessica',
                                date: '2018-10-22',
                                compeleted: true   
                            },
                            {
                                id: 13,
                                name: 'Joan',
                                date: '2018-10-01',
                                compeleted: true
                            },
                            {
                                id: 14,
                                name: 'Wang',
                                date: '2018-09-22',
                                compeleted: true
                            },
                            {
                                id: 15,
                                name: 'Kitty',
                                date: '2018-10-01',
                                compeleted: true
                            },
                            {
                                id: 16,
                                name: 'Jason',
                                date: '2018-09-22',
                                compeleted: true
                            },
                            {
                                id: 17,
                                name: 'Eva',
                                date: '2018-10-01',
                                compeleted: true
                            },
                            {
                                id: 18,
                                name: 'Kuan',
                                date: '2018-09-22',
                                compeleted: true
                            },
                            {
                                id: 12,
                                name: 'Jessica',
                                date: '2018-10-22',
                                compeleted: true   
                            },
                            {
                                id: 13,
                                name: 'Joan',
                                date: '2018-10-01',
                                compeleted: true
                            },
                            {
                                id: 14,
                                name: 'Wang',
                                date: '2018-09-22',
                                compeleted: true
                            },
                            {
                                id: 15,
                                name: 'Kitty',
                                date: '2018-10-01',
                                compeleted: true
                            },
                            {
                                id: 16,
                                name: 'Jason',
                                date: '2018-09-22',
                                compeleted: true
                            },
                            {
                                id: 17,
                                name: 'Eva',
                                date: '2018-10-01',
                                compeleted: true
                            },
                            {
                                id: 18,
                                name: 'Kuan',
                                date: '2018-09-22',
                                compeleted: true
                            },
                            {
                                id: 12,
                                name: 'Jessica',
                                date: '2018-10-22',
                                compeleted: true   
                            },
                            {
                                id: 13,
                                name: 'Joan',
                                date: '2018-10-01',
                                compeleted: true
                            },
                            {
                                id: 14,
                                name: 'Wang',
                                date: '2018-09-22',
                                compeleted: true
                            },
                            {
                                id: 15,
                                name: 'Kitty',
                                date: '2018-10-01',
                                compeleted: true
                            },
                            {
                                id: 16,
                                name: 'Jason',
                                date: '2018-09-22',
                                compeleted: true
                            },
                            {
                                id: 17,
                                name: 'Eva',
                                date: '2018-10-01',
                                compeleted: true
                            },
                            {
                                id: 18,
                                name: 'Kuan',
                                date: '2018-09-22',
                                compeleted: true
                            }
                        ]
                    },
                    pending:{
                        tableTitle:['Order', 'Id', 'Name', 'Hospitalization Date'],
                        tableContent:[
                            {
                                id: 15,
                                name: 'Ula',
                                date: '2018-08-22'
                            },
                            {
                                id: 16,
                                name: 'Edith',
                                date: '2018-09-01'
                            },
                            {
                                id: 17,
                                name: 'Cindy',
                                date: '2018-10-22'
                            } 
                        ]
                    },
                    followed:{
                        tableTitle:['Order', 'Id', 'Name', 'Department'], 
                        tableContent:[
                            {
                                id: 1,
                                name: 'Wang',
                                department: 'Kidney'
                            },
                            {
                                id: 2,
                                name: 'Chen',
                                department: 'Heart'
                            },
                            {
                                id: 3,
                                name: 'Hwang',
                                department: 'Lung'
                            },
                            {
                                id: 4,
                                name: 'Lin',
                                department: 'Foot'
                            },
                            {
                                id: 5,
                                name: 'Lu',
                                department: 'Eyes'
                            },
                            {
                                id: 6,
                                name: 'Lu',
                                department: 'Eyes'
                            }
                        ]
                    }
                },
                pagination:{}
            },
        };
      },
      created(){
      },
      mounted(){
        this.init();
      },
      methods: {
        init(){
            const {lists} = this.items;
            const keysArr = Object.keys(lists);
            const {showData} = this.items.basic;
            
            for(let i=0; i< keysArr.length; i++){

                this.items.basic.badgeCount.push(lists[keysArr[i]]['tableContent'].length);   
            }

            this.getData('incompeleted');
        }, 
        getData(category){

            const {lists} = this.items;
            const {basic} = this.items;
            const {showData} = basic;

            this.$set(showData, 'tableTitle', lists[category].tableTitle);
            this.$set(showData, 'tableContent', lists[category].tableContent);
        },
        selectedType(category){
            this.items.basic.selectedType = category;
            this.getData(category);
        },
        follow(){
            // [Follow button] effect
            const { basic } = this.items;
            basic.followBtn.clicked = !basic.followBtn.clicked;
            if(basic.followBtn.clicked){
                basic.followBtn.text = 'Followed'
            }else{
                basic.followBtn.text = 'Follow'
            } 
        },
        unfollow(list){
            //Unfollow function
            let unfollow = confirm('Sure you want to unfollow the doctor?');
        },
        // getAllData : function() {
        //     let wf = new APIClient({
        //         spec_url:"http://10.66.30.213:4000/api/spec.get",
        //         server_url:"http://10.66.30.213:4000/api"
        //     });

        //     wf.loader.then($spec => { 
        //         this.spec=$spec;
        //         this.wf=wf;
        //         console.log(wf.note);

        //         wf.note.get().then($list => {
        //             console.log($list);
        //         })

        //         wf.pt.get({no:'00000002'}).then($patient_file=> {
        //                 console.log($patient_file);
        //             }
        //         )
        //     })
        //     .catch(error => {
        //         console.log(error.response.data.message)
        //     });
        // },
      }
    }
</script>

<style lang="scss" scoped>

    @import '../assets/global.scss';
    
    .card-wrap {
        box-shadow:0 1px 8px rgba(170,170,170,.3);
        color: $brown;
        padding: 15px;
        display: flex;
        align-items: center;
        justify-content: space-between;
        @media screen and (max-width: $break-small) {
           flex-direction: column;
           padding: 10px;
        }
        span{
            font-size: 12px;
            line-height: 15px;
            font-weight: 100;
        }
        div {
            display: flex;
            align-items: center;
            justify-content: center;
            @media screen and (max-width: $break-small) {
               margin-top: 10px;
            }
            .btn {
                width: 90px;
                height: 25px;
                margin: 0 5px;
                border: 1px solid $brown;
                border-radius: 15px;
                font-size: 12px;
                font-weight: 600;
                transition: all .3s ease-in-out;
                color: $brown;
                padding: 0;
                opacity: .6;
                display: flex;
                align-items: center;
                justify-content: center;
                &:hover {
                    opacity: 1;
                }
                i{
                    color: $brown;
                    font-size: 12px;
                    margin-right: 5px;
                }
                &.followed {
                    background: $brown;
                    color: #fff;
                    opacity: 1;
                    i{
                        color: #fff;
                    }
                }
            }
        }  
    }
    .content-wrap{
        padding: 30px 50px;
        @media screen and (max-width: 1000px) {
            padding: 30px;
        }
        @media screen and (max-width: $break-medium) {
            padding: 15px;
        }
    }

    .expanded {
        margin-left: 190px;
        @media screen and (max-width: $break-medium) {
            margin-left: 0;
        }
    }

</style>
