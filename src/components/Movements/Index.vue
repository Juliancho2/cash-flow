<template>

    <div class="movements">
       <h1 class="title">Historial</h1>

       <div class="content">
          <Movement 
          v-for="movement in movements"  
          :item="movement" 
          :key="movement.id"
          @remove="remove"
          />
          
          
       </div>
    </div>
    
</template>

<script setup>
import { toRefs } from 'vue';
import Movement from './Movement.vue';

const props= defineProps({
    movements: {
      type:Array,
      default:()=>[],
    }
  });

const {movements} = toRefs(props);

const emit=defineEmits(["remove"])
const remove = (id)=>{
  emit("remove",id)
}

</script>

<style scoped>
    .movements {
      max-height: 100%;
      padding: 0 8px;
      margin-bottom: 14px;
    }
    
    .title {
      margin: 8px 16px 24px 16px;
      color: var(--brand-blue);
    }
    
    .content {
      max-height: 68vh;
      display: flex;
      flex-direction: column;
      gap: 8px;
      overflow-y: scroll;
    }
    </style>