<template>
    <section class="score">
      Acertos <span>{{ this.win }}</span> x 
     <span>{{ this.lose }}</span> Erros
     
    </section>
    <br/>
    <div>

    </div>
</template>

<script>

    export default {
        name: 'ScoreBoard',
        props: ['win', 'lose'],
    }


</script>

<style lang="scss">

   .score{
    font-weight: bold;

    span{
        border: 1px solid black;
        padding: 5px;
    }
   }
   .clear{
        padding: 2px;
        width: 80px;
        border-radius: 5px;
        background-color: inherit;
        border: inherit;
        color: black;
    }
    .clear:hover{
        color:red;
    }

  
</style>