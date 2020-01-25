<template>
    <div class="product-page-description-section-wrapper">
        <div class="product-page-description-section">
            <div class="swatch"
                v-for="option in options"
                    :key="`${option.id}`"
                >
                <div class="product-page-description-label swatch-option">
                    <span class="swatch-option__name">{{ option.name }}</span> 
                    <span 
                        class="swatch-option__value "
                        :class="[`swatch-option__value--${currentVal(option.name).split(' ').join('-')}`]"
                        >
                        {{ currentVal(option.name) }}</span>
                </div>
                <div class="swatch-block-wrapper">
                    <div 
                        v-for="value in option.values"
                            :key="`${option.name}-${value.value}`"
                            class="swatch-block"
                            :class="[
                                `swatch-block--${value.value.split(' ').join('-')}`,
                                {'active-el': value.value === currentVal(option.name)}
                            ]"
                            @click="selectOption(option.name, value.value)"
                        >
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<style lang="scss" scoped>    

    $color-black: black;
    $color-vader: #202020;
    $color-tort: #663333;
    $color-tobacco: #6D5843;
    $color-matte-olive: #808000;
    $color-polarized: blue;
    $color-non-polarized: lightgray;

    $colors: 'black' 'vader' 'tort' 'tobacco' 'matte-olive' 'polarized' 'non-polarized';

    // $colors: $color-black $color-vader $color-tort $color-tobacco $color-matte-olive $color-polarized $color-non-polarized;
    
    .swatch-option__value {
        &--black{color: $color-black}
        &--vader{color: $color-vader}
        &--tort{color: $color-tort}
        &--tobacco{color: $color-tobacco}
        &--matte-olive{color: $color-matte-olive}
        &--polarized{color: $color-polarized}
        &--non-polarized {color: $color-non-polarized}
    }
    .swatch-block {
        &--black{background-color: $color-black}
        &--vader{background-color: $color-vader}
        &--tort{background-color: $color-tort}
        &--tobacco{background-color: $color-tobacco}
        &--matte-olive{background-color: $color-matte-olive}
        &--polarized{background-color: $color-polarized}
        &--non-polarized {background-color: $color-non-polarized}
    }

    .active-el {
        outline: 3px solid #4c9ec3;
        outline-offset: 4px;

        &:hover {
            outline: 3px solid #27b2f3;
        }
    }

    .product-page{
        &-description{
            &-block{
                &-wrapper{
                    padding: 15px;
                }
            }
            &-label{
                font-size: 1.3em;
            }
            &-section{
                margin: 20px 0;
                &-wrapper{
                    margin-top:20px;
                }
            }
        }
    }
    
    .swatch{
        &-option {
            &__name {
                font-weight: 600;
            }
            &__value {
                font-weight: 300;
            }
        }
        &-block{
            height:30px;
            width:30px;
            border:rgb(185, 185, 185) solid .5px;
            border-spacing: 10px;
            margin:15px 15px 15px 0px;
            position: relative;

            &:hover{
                cursor: pointer;
                opacity: .8;
            }
        }
        &-block-wrapper{
            display: flex;
            flex-wrap: wrap;
        }
    }

    // $colors: (
    //     "black": 'black',
    //     "vader": '#202020',
    //     "tort": '#663333',
    //     "tobacco": '#6D5843',
    //     "matte-olive": '#808000',
    //     "polarized": 'blue',
    //     "non-polarized": 'lightgray'
    // );

    // @function color-variation($color) {
    //     @if map-has-key($colors, $color) {
    //         @return map-get($colors, $color);
    //     }
    //     @error "Invalid color name: `#{$color}`.";
    // }

    // .black-swatch{background-color: map-get($colors, "black")}
    // .vader-swatch{background-color: color-variation("black")}
    // .tort-swatch{background-color: color-variation("black")}
    // .tobacco-swatch{background-color: color-variation("black")}
    // .matte-olive-swatch{background-color: color-variation("black")}
    // .polarized-swatch{background-color: color-variation("black")}
    // .non-polarized-swatch {background-color: color-variation("black")}
</style>

<script>

    export default {
        name: "ProductDetails",
        components: {},
        props: {
            options: {
                type: Array, 
                required: false
            },
            selectedOptions: {
                type: Array,
                required: false
            }
        }, 
        data(){return {}},
        methods: {
            currentVal(option){return this.selectedOptions.find((swatch)=>swatch.name === option).value},
            selectOption(name, value){
                let currentOptions = {};
                for(const option of this.selectedOptions){
                    currentOptions[option.name] = option.value
                };
                debugger; 
                currentOptions[name] = value;
                this.$emit('selectOption', currentOptions);
            }
        }
    }

</script>