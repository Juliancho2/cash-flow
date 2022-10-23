<template>
    <Layout>
        <template #header>
            <Header></Header>
        </template>

        <template #resume>
            <Resume 
            :label="'Ahorror total'" 
            :label-specific="labelSpecific" 
            :amount="amount" 
            :total-amount="totalAmount">
                <template #graphic>
                    <Graphic 
                    :amounts="amounts"
                    @select="select"
                    />
                </template>
                <template #action>
                    <Action @create="create"/>
                </template>
            </Resume>
        </template>

        <template #movements>
            <Index 
            :movements="movements" 
            @remove="remove"
            />
        </template>
    </Layout>
</template>
<script>
import Header from './Header.vue';
import Layout from './Layout.vue';
import Resume from './Resume/Index.vue';
import Index from './Movements/Index.vue'
import Action from './Action.vue';
import Graphic from './Resume/Graphic.vue';
import { computed } from '@vue/reactivity';

export default {
    components: {
        Layout,
        Header,
        Resume,
        Index,
        Action,
        Graphic
    },
    data() {
        return {
            amount: null,
            labelSpecific: `Fecha: ${new Date().toLocaleDateString()}`,

            movements: []
        }

    },
    computed: {
        amounts(){

            const lastDay= this.movements
                .filter((el) => {
                    const today = new Date();
                    const oldDate = today.setDate(today.getDate() - 30);
                    
                    return el.time > oldDate;

                }).map((el) => el.amount)
            return lastDay.map((el,i)=>{
                const lastMovements= lastDay.slice(0,i + 1);

                return lastMovements.reduce((acc,item)=>{
                    return acc + item;
                },0)
            });
        },
        totalAmount(){
            return this.movements.reduce((suma,m)=>{
               return suma + m.amount; 
            },0)
        }
    },
    mounted(){
        const movements= JSON.parse(localStorage.getItem("movimientos"));

        if(Array.isArray(movements)){
            this.movements = movements.map(el=>{
            return {...el, time: new Date(el.time)}
        });
    }
        

    },
    methods:{
        create(movement){
            this.movements.push(movement);
            this.save();
        },
        remove(id){
            this.movements.splice(id,1);
            this.save();
        },
        save(){
            localStorage.setItem("movimientos",JSON.stringify(this.movements));
        },
        select(value){
            
            this.amount = value;
        }
    }
}

</script>