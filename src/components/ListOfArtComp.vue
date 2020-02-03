<template>
    <div class="row d-flex justify-content-left align-items-center">
        <h2 
            v-bind:class="[
                {'is-completed':musculationProps.learned},
                'col-4'
            ]">
                {{musculationProps.articleName}}
        </h2>
        <div class="col-8 text-right">
            <button 
                v-on:click="updateArtEmit()"
                class="btn btn-info mr-2">
                    Edit
            </button>
            <button 
                v-on:click="deleteArtEmit()" 
                class="btn btn-danger">
                    Delete
            </button>
        </div>
    </div>
</template>

<script>
export default {
    props: ["musculationProps"],
    methods: {
        deleteArtEmit(){
            this.$emit('deleteArt', this.musculationProps.id)
        },
        updateArtEmit(){
            this.$emit('updateArt', this.musculationProps)
        }
    }
}
</script>

<style>
    h2{
        font-size: 20px;
    }
    .is-completed{
        position: relative;
    }
    .is-completed:after{
        content: "L";
        color: red;
        border: 1px solid red;
        position: absolute;
        top: 50%;
        right: 15px;
        transform: translateY(-50%);
        width: 25px;
        text-align: center;
        border-radius: 100%;
    }
</style>