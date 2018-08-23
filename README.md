# uploadfilevue

- npm i uploadfilevue
  基于 vue 上传文件组件

### 参数

- accept：选择文件的类型(非必须)
- other_data:附加的数据(非必须) json 形式
- up_load_url：上传的地址（必须有）
- close_upload_plug: 回掉函数 ,参数（data:{flage,success} success:true 表示上传成功了

#### 导入进来

import UploadFile from 'uploadfilevue'
<UploadFile accept="application/vnd.openxmlformats-officedocument.spreadsheetml.sheet" @close_upload_plug='close_up_file_dialog' :up_load_url="excleUpLoadUrl"></UploadFile>
