<template>
    <div>
        <div class="product-page-wrapper">
            <div class="product-page-container"
                v-if="isLoaded"
                > 
                <div class="product-page-image-block-wrapper">
                    <image-picker 
                        :images="modImages"
                        :variant="selectedVariant"
                        v-on:selectImage="selectVariantByImage"
                        />
                </div>
                <div class="product-page-description-block-wrapper">
                    <product-swatch-picker 
                        :options="productData.options"
                        :selectedOptions="selectedVariant.selectedOptions"
                        v-on:selectOption="selectVariantBySwatch"
                        />
                    <div class="product-page-description-section">
                        <price-display
                            :price="selectedVariant.price"
                            />
                    </div>
                    <div class="product-page-description-block-wrapper">
                        <div class="product-page-description-section">
                            <quantity-picker 
                                :style="[quantityStyles]"
                                :quantity="quantity"
                                v-on:quanChange="updateQuan"
                                />
                        </div>
                        <div class="product-page-description-section">
                            <primary-button
                                @buttonClick="addVariantToCart"
                                class="atc"
                                text="ADD TO CART">
                            </primary-button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<style lang="scss" scoped>
    .product-page{
        &-wrapper{
            display: flex;
            justify-content: center
        }
        &-container{
            max-width: 1400px;
            width: 100%;
            display: grid;
            grid-template-columns: 50% 50%;
        }
        &-featured-image{
            height: auto;
            width: 100%;
        }
        &-description{
            &-block{
                &-wrapper{
                    padding: 15px;
                }
            }
            &-label{
                font-weight: 600;
            }
            &-section{
                margin-bottom: 20px;
                &-wrapper{
                    margin-top:20px;
                }
            }
        }
    }
</style>

<script>
    import ProductSwatchPicker from '../product/productSwatchPicker';
    import { PrimaryButton, ImagePicker, PriceDisplay, QuantityPicker } from '../../shared/components.js';


    import {CartService} from '../../core/services/cart.service';
    const cartService = new CartService();


    export default {
        name: "ProductVariantPicker",
        components: {     
            ImagePicker,
            ProductSwatchPicker,
            PriceDisplay,
            QuantityPicker,
            PrimaryButton
        },
        props: {
            productData: {
                type: Object,
                required: true
            }
        },
        data(){
            return {
                variants: {},
                selectedVariant: {},
                imageSelectProp: 'Color',
                isLoaded: false,
                quantityStyles: {},
                quantity: 0
            }
        },
        created(){
            const {variants} = this.productData;
                this.variants = variants;
                this.selectedVariant = variants[0];
            this.isLoaded = true;
            // quantity picker
            this.quantityStyles = {'--color-hover':'#27b2f3'}
        },
        computed: {
            modImages(){
                return this.productData.images.map((img)=>{
                    let el = document.createElement('img');
                    el.src = img.src;
                    img['width'] = el.width;
                    img['length'] = el.length;
                    return img;
                })
            }
        },
        methods: {
            addVariantToCart(){
                const variantID = this.selectedVariant.id;
                const qty = this.quantity;
                debugger;
                cartService.addItem(qty,variantID).then(()=>{
                    shopserv.load(false);
                })
            },
            selectVariantByImage(imgId){
                // collect variants that use the selected image
                let variantsMatchingImage = this.variants.filter((variant)=>variant.image.id === imgId);
                // collect other currently selected options EXCEPT for the option that the image selects for (Color)
                let otherMatchOptions = this.selectedVariant.selectedOptions.filter((option)=>{return option.name != this.imageSelectProp})
                // find the variant that matches all the other currently selected options
                let selectedVariant = variantsMatchingImage.filter(function(variant){
                    return otherMatchOptions.every(function(matchOption){
                        return variant.selectedOptions.find(function(potentialMatchOption){
                            return potentialMatchOption.name == matchOption.name && potentialMatchOption.value == matchOption.value})
                    })
                })
                this.selectedVariant = selectedVariant[0];
            },  
            selectVariantBySwatch(activeSwatches){
                let selectedVariant = this.variants.find(function (variant) {
                    return variant.selectedOptions.every(function (option) {
                        return activeSwatches[option.name] === option.value;
                    });
                })
                this.selectedVariant = selectedVariant;
            },
            updateQuan(quan){
                this.quantity = quan;
            }
        }
    }
</script>