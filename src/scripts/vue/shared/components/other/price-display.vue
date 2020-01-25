<template>
    <div class="price-container">
        <div class="price-container__price">{{ price }}</div>
        <!-- <div class="price-container__promo">
            Promo
        </div> -->
    </div>
</template>

<style lang="scss" scoped>

    // * {
    //     border: 1px solid red;
    // }

</style>

<script>
export default {
    name: "ProductPrice",
    props: ["price"],
    data(){
        return {}
    }
}
</script>


