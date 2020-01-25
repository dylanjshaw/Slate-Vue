<template>
    <div class="quantity">
        <svg
            @click="decrement" 
            class="quantity__sign"
                @mousedown="startDecrement" 
                @mouseleave="stop" 
                @mouseup="stop" 
                @touchstart="startDecrement" 
                @touchend="stop"
                @touchcancel="stop" 
            version="1.1" xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 32 32">
            <title>minus</title>
            <path d="M0 13v6c0 0.552 0.448 1 1 1h30c0.552 0 1-0.448 1-1v-6c0-0.552-0.448-1-1-1h-30c-0.552 0-1 0.448-1 1z"></path>
        </svg>
        <div
            class="quantity__value"
            >{{ dataQuantity }}</div>
        <svg
            @click="increment" 
            class="quantity__sign"
                @mousedown="startIncrement" 
                @mouseleave="stop" 
                @mouseup="stop" 
                @touchstart="startIncrement" 
                @touchend="stop"
                @touchcancel="stop" 
            version="1.1" xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 32 32">
            <title>plus</title>
            <path d="M31 12h-11v-11c0-0.552-0.448-1-1-1h-6c-0.552 0-1 0.448-1 1v11h-11c-0.552 0-1 0.448-1 1v6c0 0.552 0.448 1 1 1h11v11c0 0.552 0.448 1 1 1h6c0.552 0 1-0.448 1-1v-11h11c0.552 0 1-0.448 1-1v-6c0-0.552-0.448-1-1-1z"></path>
        </svg>
    </div>
</template>

<style lang="scss" scoped>

.quantity {
    // prevent highlighting on double click
    -webkit-user-select: none; /* Chrome/Safari */        
    -moz-user-select: none; /* Firefox */
    -ms-user-select: none; /* IE10+ */

    &__sign {
        width: 1rem;
        height: 1rem;
        transition: all .1s;

        &:hover {
            fill: var(--color-hover);
            cursor: pointer;
            transform: scale(1.2);
        }
    }

    &__value {
        height: 30px;
        width: fit-content;
        font-size: 1.5rem;
        display: inline-flex;
        justify-content: center;
        align-items: center;
        margin: 0 5px;
    }

}

</style>

<script>
    export default {
        name: "quantityCounter",
        props: {
            quantity: {
                type: Number,
                required: false
            }
        },
        data(){
            return {
                dataQuantity : 0,
                interval: false,
            }
        },
        created(){
            this.dataQuantity = this.quantity || 0;
        },
        methods: {
            // handle single click
            increment(){this.dataQuantity++;},
            decrement(){if(this.dataQuantity > 0){this.dataQuantity--}},
            // handle click + hold
            startIncrement(){if(!this.interval){this.interval = setInterval(() => this.dataQuantity++, 200)}},
            startDecrement(){if(!this.interval){this.interval = setInterval(() => this.dataQuantity--, 200)}},
            stop(){clearInterval(this.interval);this.interval = false;}
        },
        watch: {
            dataQuantity:{
                handler(){this.$emit('quanChange', this.dataQuantity);}
            }
        }
    }
</script>