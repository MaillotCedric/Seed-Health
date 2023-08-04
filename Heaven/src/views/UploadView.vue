<template>
    <div class="upload">
        <v-file-input
            multiple
            label="File input"
            @change="onFileChange"
        ></v-file-input>
    </div>
</template>

<script lang="ts">
    import * as tb from '../scripts/toolbox'
    import axios from 'axios'

    export default {
        data() {
            return {
                nbFilesUploaded: 0
            }
        },
        methods: {
            onFileChange(event) {
                const formData = new FormData()

                for (let index = 0; index < event.target.files.length; index++) {
                    const file = event.target.files[index];
                    
                    formData.append("file", file)
                    axios
                    .post("api/images/", formData)
                    .then(() => {
                        // console.log(index)
                        // console.log("SUCCESS")
                        this.nbFilesUploaded++
                        if (this.nbFilesUploaded === event.target.files.length) {
                            tb.print_("DONE")
                        }
                    })
                    .catch(() => {
                        console.log("FAILED");
                    });
                }
            }
        }
    }
</script>

<style>
    @media (min-width: 1024px) {
        .upload {
            min-height: 100vh;
            display: flex;
            align-items: center;
        }
    }
</style>
