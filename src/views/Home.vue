<template>
    <div>
        <h1 class="text-center font-weight-light my-5">File Browser</h1>
        <v-row>
            <v-col>
                <file-browser
                    
                    :maxUploadFileSize="5242880"
                    :maxUploadFilesCount="5"
                    :storages="'local'"
                />
            </v-col>
        </v-row>
    </div>
</template>

<script>
import FileBrowser from "vuetify-file-browser";

export default {
    components: {
        FileBrowser
    }
};
</script>
