<template>
    <div>
        <svg

            @touchstart="tap"
            @touchmove="tap"
            @touchend="untap"

            viewBox="0 0 300 200"
        >
            <line
                stroke="#c4c4c4"
                stroke-width="2"
                x1="0"
                :y1="zero"
                x2="300"
                :y2="zero"
            />
            <polyline
                fill="none"
                stroke="#0689B0"
                stroke-width="2"
               :points="points"
            />
            <line
                v-show="showPointer"
                stroke="#04b500"
                stroke-width="2"
                :x1="pointer "
                y1="0"
                :x2="pointer "
                y2="200"
            />
        </svg>

        <p>Ultimos 30 dias</p>
        
    </div>
   
    
</template>
<script setup>
import { def } from '@vue/shared';
import { computed, toRefs,defineProps,defineEmits, ref,watch } from 'vue';

    const props= defineProps({
        amounts:{
            type:Array,
            default:()=>[],
        }
    });
const {amounts} = toRefs(props);



const AmountToPixels=(amount)=>{

    const  min=Math.min(...amounts.value);
    const max=Math.max(...amounts.value);

    const amountAbs= amount + Math.abs(min);
    const minmax= amount + Math.abs(max ) +  Math.abs(min);

    return 200 - ((amountAbs * 100) / minmax * 2);
    
};

const zero= computed(()=>{
    return AmountToPixels(0);
});

const points=computed(()=>{
    const total= amounts.value.length;
    
    return amounts.value.reduce((acc,item,index)=>{
        const x=300/ total * (index + 1),
        y= AmountToPixels(item);

        

        return `${acc} ${x},${y}`;
    },`0 , ${AmountToPixels(amounts.value.length ? amounts.value[0] : 0)}`);
});
const showPointer= ref(false);
const pointer= ref(0);
watch(pointer,(value)=>{
    const index= Math.ceil((value / ( 300 / amounts.value.length) ));

    if(index < 0 || index  > amounts.value.length) return;
    emit("select",amounts.value[index - 1]);

});

const emit= defineEmits(["select"]);

const tap = ({target, touches})=>{
    showPointer.value= true;

    const elementsWidth= target.getBoundingClientRect().width;
    const elementsX= target.getBoundingClientRect().x;
    const touchX= touches[0].clientX;

    pointer.value=((touchX - elementsX) * 300) / elementsWidth;
    emit("select" )
}
const untap=()=>{
    showPointer.value= false;
}

</script>

<style scoped>
    svg {
      width: 100%;
    }
    
    p {
      text-align: center;
    }
</style>