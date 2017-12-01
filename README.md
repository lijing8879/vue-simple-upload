# vue-simple-file-upload
a simple vue file upload component use XHR

> vue file upload component use XHR


### 1. Install

``` bash
npm install vue-simple-file-upload
```


#### 2. Usage

```
import VueSimpleUpload from 'vue-simple-file-upload'
Vue.use(VueSimpleUpload)
...

<template>
  <div>
    <vue-simple-upload :options="options" v-on:progress-update="progressUpdate">
    </vue-simple-upload>
  </div>
</template>
<script>
export default {
  data() {
    return {
      options: {
        className: 'btn-solid',
        btnContent: 'Click Me',
        url: '/api/files/upload',
        accept: 'image/png'
      },
      imageUrl: ''
    }
  },

  methods: {
    progressUpdate(fileInfoList) {
      console.log("upload speed（kb/s）：", fileInfoList[0].uploadSpeed)
      if (fileInfoList[0].type === 'success') {
        // do something
      }
    }
  }
}
</script>
```

#### 3. options

* **url**
  * file uploading request url
  * type **String**
  * required **Yes**
* **btnContent:**
  * 'choose file' botton content
  * type **String**
  * required **No**
  * default **'Choose File'**
* **className:**
  * 'choose file' botton class name
  * type **String**  ps:if you have multiple className, use space split them
  * required **No**
  * default **''**
* **accept**
  * file type accepted
  * type **String**  ps:same as the 'accept' attribute in \<input\>
  * required **No**
  * default **'\*'**
* **multiple**
  * multiple file upload
  * type **Boolean**
  * required **No**
  * default **false**


#### 4. progress-update

I use an '$emit' function to send upload message to parent component.And this function has just one argument : fileList (an Array)

It will be look like this blow:
```
[
  {
    fileInfo: File // the first file you choose.
    fileName // the first file name you choose.
    progress // uploaded progress. eg: '15.01%'
    uploadSpeed // upload speed. eg: '1001'(kb/s)
    type // uploading type, could be 'waiting', 'uploading', 'success' or 'fail' or 'abort'
    id // an unique string, for uploading abort (todo)
    response // upload requset response (if your file is uploading, this would be an empty Object)
  },
  ... // more elements if you are uploading multiple files
]
```


#### 5. abort

You can abort uploading request by using ```this.$refs.XXX.abort()```

eg:

```
<vue-simple-upload :options="options" v-on:progress-update="progressUpdate" ref="fileUploadSection">
</vue-simple-upload>

...
methods: {
  ...
  abort(id) {
    this.$refs.fileUploadSection.abort() // abort all files which are uploading
    this.$refs.fileUploadSection.abort(id) // abort one file by fileInfoList.id
  }
  ...
}

```

// 2017-12-01 todo: custome start 、 drag upload.

