

<template>
    <div class="movement">
       <div class="content">
            <h3>{{item.title}}</h3>
            <p>{{item.description}}</p>
       </div>
       <div class="action">
            <img src="@/assets/trash-icon_14040a31-4162-43fa-bb4e-f5f86a26eb86.svg" alt="Delete" @click="remove(item)">
            <p :class="isNegative">{{amountCurrency}}</p>
       </div>
    </div>
</template>

<script setup>

import { toRefs,computed,defineEmits } from 'vue';

const props = defineProps({
    item: {
        type: Object,
    },
    amount:{
        type:Number,
    }

});
const { item } = toRefs(props);
const emit= defineEmits(["remove"]);

const currencyFormatter= new Intl.NumberFormat("es-CO",{
        style:"currency",
        currency:"COP",
    });

const amountCurrency=computed(()=>{
    return currencyFormatter.format(item.value.amount)
})
const remove= (item)=>{
    emit("remove",item.id)
}
const isNegative= computed(()=>{
   return item.value.amount < 0? 'red':'green'
}
);

</script>

<style scoped>
    .movement {
      display: flex;
      justify-content: space-between;
      align-items: center;
      width: 100%;
      padding: 16px;
      background-color: #e6f9ff;
      border-radius: 8px;
      box-sizing: border-box;
    }
    .movement .content {
      width: 100%;
    }
    .movement .action {
      display: flex;
      justify-content: space-between;
      align-items: flex-end;
      flex-direction: column;
    }
    h4,
    p {
      margin: 0;
      padding: 0;
    }
    h3 {
      margin-bottom: 8px;
    }
    .movement .action img {
      margin-bottom: 16px;
    }
    .red {
        color: red;
    }
    .green {
        color: green;
    }
    </style>