---
title: angular-file-upload
date: 2017-08-14 18:14:25
tags:
---

背景
----
今天做项目遇到文件上传问题，于是找到了一个很强大的文件上传插件angular-file-upload,便做了下使用总结，方便给为老铁查阅啊。  

安装
----

    npm install --save-dev angularFileUpload  

使用
----
demo1:多文件上传-html文件：


    <input type="file" id="file" name="file" nv-file-select uploader="uploader" ng-click="clearItems()" multiple> 上传

更酷的界面效果可以这样用，在htm文件加入官网的例子：

    <div class="col-md-9" style="margin-bottom: 40px">
                <h2>Uploads only images (with canvas preview)</h2>
                <h3>The queue</h3>
                <p>Queue length: {{ uploader.queue.length }}</p>

                <table class="table">
                    <thead>
                        <tr>
                            <th width="50%">Name</th>
                            <th ng-show="uploader.isHTML5">Size</th>
                            <th ng-show="uploader.isHTML5">Progress</th>
                            <th>Status</th>
                            <th>Actions</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr ng-repeat="item in uploader.queue">
                            <td>
                                <strong>{{ item.file.name }}</strong>
                                <!-- Image preview -->
                                <!--auto height-->
                                <!--<div ng-thumb="{ file: item.file, width: 100 }"></div>-->
                                <!--auto width-->
                                <div ng-show="uploader.isHTML5" ng-thumb="{ file: item._file, height: 100 }"></div>
                                <!--fixed width and height -->
                                <!--<div ng-thumb="{ file: item.file, width: 100, height: 100 }"></div>-->
                            </td>
                            <td ng-show="uploader.isHTML5" nowrap>{{ item.file.size/1024/1024|number:2 }} MB</td>
                            <td ng-show="uploader.isHTML5">
                                <div class="progress" style="margin-bottom: 0;">
                                    <div class="progress-bar" role="progressbar" ng-style="{ 'width': item.progress + '%' }"></div>
                                </div>
                            </td>
                            <td class="text-center">
                                <span ng-show="item.isSuccess"><i class="glyphicon glyphicon-ok"></i></span>
                                <span ng-show="item.isCancel"><i class="glyphicon glyphicon-ban-circle"></i></span>
                                <span ng-show="item.isError"><i class="glyphicon glyphicon-remove"></i></span>
                            </td>
                            <td nowrap>
                                <button type="button" class="btn btn-success btn-xs" ng-click="item.upload()" ng-disabled="item.isReady || item.isUploading || item.isSuccess">
                                    <span class="glyphicon glyphicon-upload"></span> Upload
                                </button>
                                <button type="button" class="btn btn-warning btn-xs" ng-click="item.cancel()" ng-disabled="!item.isUploading">
                                    <span class="glyphicon glyphicon-ban-circle"></span> Cancel
                                </button>
                                <button type="button" class="btn btn-danger btn-xs" ng-click="item.remove()">
                                    <span class="glyphicon glyphicon-trash"></span> Remove
                                </button>
                            </td>
                        </tr>
                    </tbody>
                </table>

                <div>
                    <div>
                        Queue progress:
                        <div class="progress" style="">
                            <div class="progress-bar" role="progressbar" ng-style="{ 'width': uploader.progress + '%' }"></div>
                        </div>
                    </div>
                    <button type="button" class="btn btn-success btn-s" ng-click="uploader.uploadAll()" ng-disabled="!uploader.getNotUploadedItems().length">
                        <span class="glyphicon glyphicon-upload"></span> Upload all
                    </button>
                    <button type="button" class="btn btn-warning btn-s" ng-click="uploader.cancelAll()" ng-disabled="!uploader.isUploading">
                        <span class="glyphicon glyphicon-ban-circle"></span> Cancel all
                    </button>
                    <button type="button" class="btn btn-danger btn-s" ng-click="uploader.clearQueue()" ng-disabled="!uploader.queue.length">
                        <span class="glyphicon glyphicon-trash"></span> Remove all
                    </button>
                </div>

            </div>

        </div>



在项目中引入依赖：

    import angularFileUpload from 'angular-file-upload';

    var app = angular
        .module('app',[
          'angularFileUpload'
        ])
        .controller('myController',['FileUploader'],controller);

    function controller(FileUploader) {
        var uploader = $scope.uploader = new FileUploader({
            url: 'api/portName',
            queueAfterUpload: 1,   //文件个数
            removeAfterUpload: true  //上传后删除文件
        });

        $scope.clearItems = function(){ //重新选择文件时，清空队列，达到覆盖文件的效果
            uploader.clearQueue();
        };

        //FILTERS
        uploader.filters.push({
            name: 'imageFilter',
            fn: function(item /*{File|FileLikeObject}*/,options) {
                var type = '|' + item.type.slice(item.type.lastIndexOf('/') + 1) + '|';
                return '|jpg|png|jpeg|bmp|gif|'.indexOf(type) !==-1;
            }
        });

        //CALLBACKS
        uploader.onWhenAddingFileFailed = function(item /*{File|FileLikeObject}*/, filter, options) {
            alert('只支持图片上传！');
        };
        uploader.onAfterAddingFile = function(fileItem) {
            console.info('onAfterAddingFile',fileItem);
            $scope.fileItem = fileItem._file;   //添加文件之后，把文件信息赋给scope
            //在这里可以判断添加的文件名后缀，文件大小限制
            if($scope.fileItem.size > MaxSize) {
                alert('文件大小超限!');
                uploader.removeFromQueue(fileItem);//超过大小移出队列
            }else {
                uploader.uploadAll();
            }

        };
        uploader.onAfterAddingAll = function(addedFileItems) {
            console.info('onAfterAddingAll', addedFileItems);
        };
        uploader.onBeforeUploadItem = function(item) {
            console.info('onBeforeUploadItem', item);
        };
        uploader.onProgressItem = function(fileItem, progress) {
            console.info('onProgressItem', fileItem, progress);
        };
        uploader.onProgressAll = function(progress) {
            console.info('onProgressAll', progress);
        };
        uploader.onSuccessItem = function(fileItem, response, status, headers) {
            console.info('onSuccessItem', fileItem, response, status, headers);
            //判断是否上传成功
            if(response.status == 1) {
                alert('上传成功！')
            }
        };
        uploader.onErrorItem = function(fileItem, response, status, headers) {
            console.info('onErrorItem', fileItem, response, status, headers);
        };
        uploader.onCancelItem = function(fileItem, response, status, headers) {
            console.info('onCancelItem', fileItem, response, status, headers);
        };
        uploader.onCompleteItem = function(fileItem, response, status, headers) {
            console.info('onCompleteItem', fileItem, response, status, headers);
        };
        uploader.onCompleteAll = function() {
            console.info('onCompleteAll');
        };
    }

demo2:过滤器的使用
例子中过滤|doc|docx|jpg|png|pdf|多种文件格式。
html:

    <input type="file" id="file" name="file" nv-file-select uploader="uploader" ng-click="clearItems()" filters="nameFilter"> 上传

controller.js:

    // FILTERS
    uploader.filters.push({
        name: 'nameFilter',
        fn: function(item /*{File|FileLikeObject}*/, options) {
            var type = '|' + item.name.slice(item.name.lastIndexOf('.') + 1) + '|';
            return '|doc|docx|jpg|png|pdf|'.indexOf(type) !== -1;
        }
    });

多个过滤器使用
html:

    <input type="file" id="file" name="file" nv-file-select uploader="uploader" ng-click="clearItems()" filters="filtername1,filtername2"> 上传

    // FILTERS
    uploader.filters.push({
        name: 'filtername1',
        fn: function(item /*{File|FileLikeObject}*/, options) {
            ...
        }
    },{
        name: 'filtername2',
        fn: function(item /*{File|FileLikeObject}*/, options) {
            ...
        }
    });

注意：filtername1和filtername2两个过滤器之间是逻辑与关系（即&&），所以使用的时候避免出现逻辑或。做项目写了个坑，filtername1过滤器校验图片，filtername2校验word，html使用filters="filtername1,filtername2"就出现图片和word都不允许上传问题。解决方法是用

    item.name.slice(item.name.lastIndexOf('/') + 1) + '|';
    return '|doc|docx|jpg|png|pdf|'.indexOf(type) !== -1;

小结：angular-file-uplo插件支持多文件上传，限制文件大小，将文件上传用队列queue管理，有很多API可供调用，完成业务逻辑需求，如果浏览器支持H5的话，界面效果也很酷，可以增加进度条展示等等。
