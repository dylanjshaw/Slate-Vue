<template >
    <div>
        <div class="product-page-wrapper">
            <div class="">
                <h1>{{title}}</h1>
            </div>
        </div>
        <product-variant-picker
            v-if="isLoaded"
            :productData="productData"
        ></product-variant-picker>
    </div>
</template>

<style lang="scss" scoped>

    .atc {
        outline: none;
        box-shadow: 0 2px 4px black;


        &:hover {
            transform: translateY(-3px);
            box-shadow: 0 4px 6px black;
        }

        &:active {
            transform: translateY(-1px);
            box-shadow: 0 2px 4px black;
        }
    }

    .whole-line {display: block;}

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

    hr{
        border: .5px #dad9d9 solid
    }

    h1{
        font-weight: 400;
        letter-spacing: 1.2px;
        line-height: 1.2;
    }



    .below-fold {
        text-align: center;
        padding: 300px 0;
    }

</style>


<script>
    import { ShopifyService } from '../../core/services.js';

    //sub-components
    import ProductVariantPicker from '../product/productVariantPicker';

    /* SERVICES*/
    const shopifyService = new ShopifyService();


    export default {
        // Component Bindings
        name: 'Product',
        props: {
            id: String
        },
        // Component Data
        data: function () {
            return {
                availableForSale:'',
                createdAt:'',
                description:'',
                descriptionHtml:'',
                handle:'',
                options:'',
                productType:'',
                publishedAt:'',
                title:'',
                type:'',
                updatedAt:'',
                variants:'',
                vendor:'',
                images:{},
                featuredImage:{},
                featuredPrice:'',
                lensOptions:{},
                colorOptions:{},
                conciseOptions: {},
                productData:{},
                isLoaded: false,
                quantityStyles: {}
            }
        },
        components:{ProductVariantPicker},
        methods: {},
        created: function () {
            // Runs on Init
            shopifyService.getProduct(this.id).then((resp)=>{  
                this.availableForSale= resp.availableForSale
                this.createdAt = resp.createdAt;
                this.description = resp.description;
                this.descriptionHtml = resp.descriptionHtml;
                this.handle = resp.handle;
                this.productType = resp.productType;
                this.publishedAt = resp.publishedAt;
                this.title = resp.title;
                this.type = resp.type;
                this.updatedAt = resp.updatedAt;
                this.vendor = resp.vendor;

                // variant picker
                this.productData = {
                    variants : resp.variants,
                    images : resp.images,
                    options : resp.options,
                };   

                // template can begin rendering        
                this.isLoaded = true;
           })
        }
    }

</script>



