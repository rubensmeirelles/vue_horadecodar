<template>
    <div class="main-banner">
        <h1>Faça seu Hamburguer</h1>
    </div>
</template>

<script>
export default {
    
}
</script>

<style>
    .main-banner{
        background-image: url('/img/burger.jpg');
        background-position: 0 -350px;
        background-size: cover;
        height: 700px;
        display: flex;
        justify-content: flex-start;
        align-items: center;
    }

    .main-banner h1{
        text-align: center;
        color: #fff;
        font-size: 60px;
        padding: 20px 40px;
    }
</style>