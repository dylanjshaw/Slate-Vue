<template>
    <div class="images-picker"
        v-if="isMounted"
        > 
        <div class="images-picker__selected-image">
            <zoom-on-hover 
                class="images-picker__image"
                :img-normal="variant.image.src"
                :key="variant.image.id"
                :scale="scale"
                :disabled="true"
                ></zoom-on-hover>
        </div>
        <div class="images-picker__options"
            @click="selectImage"
            v-for="img in images"
                :key="img.id"
            >
            <img 
                class="images-picker__image" 
                :class="[{'active-el': img.id === variant.image.id}]"
                :src="img.src" 
                :data-id="img.id"
            />
        </div>
    </div>
</template>

<style lang="scss" scoped>

    .active-el {
        outline: 3px solid #4c9ec3;
        outline-offset: 4px;

        &:hover {
            outline: 3px solid #27b2f3;
        }
    }

    .images-picker {
        display: flex;
        flex-wrap: wrap;
        justify-content: center;

        &__selected-image {
            flex: 0 0 100%;
        }
        &__options {
            flex: 0 0 10%;
            margin: .5em;
        }
        &__image {
            max-width: 100%;
        }
    }
</style>

<script>

    import '@intera/vue-zoom-on-hover/zoomOnHover.css'
    import ZoomOnHover from '@intera/vue-zoom-on-hover';


export default {
    name: "ImagePicker",
    components: {ZoomOnHover},
    props: {
        images: {
            type: Array,
            required: true
        },
        variant: {
            type: Object, 
            required: false
        }
    },
    data(){
        return {
            scale: 1,
            isMounted: false,
        }
    },
    mounted(){this.isMounted = true;},
    methods: {
        selectImage(){
            this.$emit('selectImage', event.target.getAttribute('data-id'));
        }
        // resetImageData(newImg){this.scale = newImg.id === this.firstSelected.id ? 1 : (this.firstSelected.dimensions.width / newImg.dimensions.width);}
    },   
    watch: {
        // dataSelectedImage: {
        //     handler(newImg, oldImg){
        //         if(this.isMounted){
        //             if(oldImg.id && oldImg.id !== newImg.id){
        //                 this.resetImageData(newImg);
        //             }
        //         }
        //     },
        //     immediate: true 
        // }
    }
}
</script>