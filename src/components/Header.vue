<template>
    <header >
        <img
            @click="reload"
            src="../assets/cashflow-icon_73798840-dd9d-4253-9c6d-821fe57c686a.svg" 
            alt="Logo">
        <h1> <span>Cash</span> Flow</h1>
        <span></span>
    </header>
</template>

<script>
    export default {
        methods:{
             reload(){
                location.reload();
             }
        }
    }
</script>

<style scoped>
  * {
    --brand-green: #04b500;
    --brand-blue: #0689b0;
}
    header {
      
      display: flex;
      justify-content: space-around;
      align-items: center;
      width: 100vw;
    }
    img,
    span {
      width: 48px;
    }
    h1 {
      font-size: 1.4rem;
      color: var(--brand-blue);
    }
    h1 span {
      color: var(--brand-green);
    }

 
    
</style>