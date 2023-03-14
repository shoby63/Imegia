 <script scope="this api replaced by slot-scope in 2.5.0+"> 
// const axios = require('axios');
export default({
    name: '#app',
    data() {
        return {
        uploaded: [],
        status: {
            0: "PENDING",
            1: "UPLOADING",
            2: "DONE",
            3: "FAILED"
        }
    }},
    methods: {
        addFileToList(event) {
            let files = event.target.files || event.dataTransfer.files;
            if (!files.length)
                return;
            for (let i = 0; i < files.length; i++) {
                let temp = {
                    "url": URL.createObjectURL(files[i]),
                    "name": files[i].name,
                    "size": files[i].size,
                    "progress": 0,
                    "status": 0,
                    "download": undefined
                }
                this.uploaded.push(temp)
                this.uploadFile(i, files[i])
            }
            
        },
        getStatus(value) {
            return this.status[value]
        },
        uploadFile(index, file) {
            let currentFile = this.uploaded[index];
            console.log(currentFile);
            const token = "OEruOpNAfkuao5ev1j6ZQEDMl48EjH0FHx6TQLoF";
            let header = {
                "Authorization":`Bearer ${token}`,
                "accept": "application/json",
                "content-type": "application/json"
            }
            const config = {
                onUploadProgress: (progressEvent)=>currentFile["progress"] = Math.round((progressEvent.loaded * 100) / progressEvent.total),
                header: header
            }
            let data = new FormData()
            data.append('file', file)
            currentFile["status"] = 1 // uploading
           
            console.log(data);
            axios.post("https://api.oyyi.xyz/v1/optimize", data, config)
                .then((r) => {
                    console.log(r)
                    if(r.status === 200){
                        currentFile["status"] = 2
                        currentFile["download"] = r.data["converted_file"];
                    }else{
                        currentFile["status"] = 3
                    }
                },)
                .catch(e => currentFile["status"] = 3)
        }
    }
})
 </script> 
<template>
    <section class="main" id="app">
        <div class="container ">
            <div class="flex flex-col items-center lg:flex-row py-10 px-4 gap-20">
                <div class="upload-section">
                    <img src="src\assets\images\upload.png" alt=""/>
                    <h6 class="text-center text-2xl font-bold">Drag and Drop files to upload</h6>
                    <h6 class="text-center font-semibold text-lg">or</h6>
                    <label for="files" class="btn-browse">Browse</label>
                    <input id="files" multiple class="file_input" type="file" @change="addFileToList" accept="application/pdf,image/webp,image/tiff,image/png,image/jpg,image/jpeg,image/wepp,">
                    <h6 class="text-center text-xs text-gray-500">Supported files: JPEG, PNG, WEBM, TIFF, PDF</h6>
                </div>
                <div class="uploaded-files-section">
                    <div class="uploaded-files">
                 <!-- <h1>  hii       {{ this.uploaded[0] }} hii </h1> -->
                        <div v-if="this.uploaded.length > 0">
                            <h4 class="font-extrabold text-3xl text-center w-full">Uploaded Files</h4>
                            <div class="file my-4" v-for="item in uploaded">
                                <div class="flex items-center">
                                    <div class="img-frame">
                                        <img :src="item['url']" class="img" alt="upload_image" onerror="this.src='src\assets\images\no-photo.png'"/>
                                    </div>
                                  <div class="flex flex-col gap-2 justify-center">
                                      <h6 class="file-name">{{item['name']}}</h6>
                                      <div id="progress" class="progress-bar">
                                          <div class="overflow-hidden text-xs flex rounded bg-orange-200" style="width: 300px; max-width: 400px;height: 20px;">
                                              <div :style="'width:'+item['progress']+'%'" class="shadow-none flex flex-col text-center whitespace-nowrap text-white justify-center bg-orange-400">
                                                  <div v-if="item['progress'] === 100">
                                                      <h6 v-if="item['status'] === 2">Done</h6>
                                                      <h6 v-else>Processing</h6>
                                                  </div>
                                                  <div v-else>
                                                      <h6>{{item['progress']}}</h6>
                                                  </div>
                                              </div>
                                          </div>
                                      </div>
                                  </div>
                                </div>
                                <div class="flex gap-x-4" v-if="getStatus(item['status']) === 'DONE'">
                                    <a :href="item['download']" download target="_blank"><img src="src\assets\images\download.png" alt="download_btn"></a>
                                </div>
                                <div class="flex items-center gap-x-4" v-if="getStatus(item['status']) === 'FAILED'">
                                    <h6 class="text-red-600">Failed</h6>
                                    <img src="src\assets\images\failed.png" alt="">
                                </div>
                            </div>
                            <button class="btn">Download All</button>
                        </div>
                      <div v-else>
                          <img src="src\assets\images\nofiles.svg" class="w-5/12 mx-auto" alt="no file choosen">
                          <h6 class="no-files">No Files Uploaded</h6>
                      </div>
                    </div>

                </div>
            </div>

        </div>
    </section>

</template>
<style scoped>
.main {
    min-height: calc(100vh - 100px);
    display: flex;
    justify-content: center;
    align-items: center;
    @apply my-10 mx-2 md:mx-10
}

.upload-section {
    position: relative;
    @apply flex flex-col items-center gap-y-5 h-3/4 lg:w-2/5 px-10 py-10 bg-orange-100 bg-opacity-30 border-4 border-orange-200 border-dashed rounded-3xl
}

.uploaded-files-section {
    @apply flex flex-col items-center lg:w-1/2
}

.uploaded-files {
    @apply flex w-full h-96 flex-col my-10 gap-y-8 overflow-auto 
}

.no-files {
    @apply text-center text-xl font-semibold text-gray-500 
}

.file {
    @apply flex py-2 px-3 border bg-gray-100 bg-opacity-30 rounded-2xl w-full items-center justify-between
}

.img-frame {
    @apply flex bg-gray-100 h-12 w-12
}

.img {
    @apply w-auto h-10 mx-auto my-auto
}

.btn-browse {
    @apply bg-orange-400 hover:bg-orange-500 text-white font-bold py-2 px-4 rounded-full text-center text-xl
}

.file-name {
    @apply font-semibold text-base ml-5
}

.progress-bar {
    @apply w-4/5 ml-5
}

.btn {
    @apply bg-black hover:bg-orange-500 text-white font-bold py-2 px-4 w-36 mx-4 rounded-full
}
</style>