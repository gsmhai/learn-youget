# learn-youget

main(**kwargs)   __main__.py

||
 main(**kwargs)   common.py
 
 ||
 script_main(any_download, any_download_playlist, **kwargs)
 ||
 
 ### | |->   any_download(url, **kwargs)  
 
        根据传入的url,查找 SITES{}， 引入./extractors 目录下的对应视频网站名称的模块
 
         m.download(url, **kwargs)
 
        调用相应模块的 download 函数
 
 |
 
 ### |->    any_download_playlist(url, **kwargs)
 
        根据传入的url,查找 SITES{}， 引入./extractors 目录下的对应视频网站名称的模块
        
        |-> m.download_playlist(url, **kwargs)
        
        调用相应模块的 download_playlist 函数
        
     
     
# 主要功能实现  ： common.py
### download_urls(urls, title, ext, total_size, output_dir='.', refer=None, merge=True,faker=False, headers={}, **kwargs)

  urls: 下载网址，一般是有第三方网址的模块提供过来的视频分片段列表
  
  title:下载视频名称，一般是有第三方网址模块提供过来的
  
  ext: 视频文件扩展名
  
  total_size： 文件总大小
  
  output_dir：保存的文件目录
  
  merge： 是否合并为一个总的视频文件
  
  faker: 是否采用模拟head
  
  headers： 请求的headers

  kwargs： 关键字参数有：
          av: 合并文件的时候调用 ffmpeg_concat_av 来合并视频片段 （download_urls）
          
          part:视频片段编号 （download_urls->get_output_filename）
          
          m3u8/m3u8_url:
          
          m3u8_type:     
          
          flush:
          # learn-youget

main(**kwargs)   __main__.py

||
 main(**kwargs)   common.py
 
 ||
 script_main(any_download, any_download_playlist, **kwargs)
 ||
 
 ### | |->   any_download(url, **kwargs)  
 
        根据传入的url,查找 SITES{}， 引入./extractors 目录下的对应视频网站名称的模块
 
         m.download(url, **kwargs)
 
        调用相应模块的 download 函数
 
 |
 
 ### |->    any_download_playlist(url, **kwargs)
 
        根据传入的url,查找 SITES{}， 引入./extractors 目录下的对应视频网站名称的模块
        
        |-> m.download_playlist(url, **kwargs)
        
        调用相应模块的 download_playlist 函数
        
     
     
# 主要功能实现  ： common.py
### download_urls(urls, title, ext, total_size, output_dir='.', refer=None, merge=True,faker=False, headers={}, **kwargs)

  urls: 下载网址，一般是有第三方网址的模块提供过来的视频分片段列表
  
  title:下载视频名称，一般是有第三方网址模块提供过来的
  
  ext: 视频文件扩展名
  
  total_size： 文件总大小
  
  output_dir：保存的文件目录
  
  merge： 是否合并为一个总的视频文件
  
  faker: 是否采用模拟head
  
  headers： 请求的headers

  kwargs： 关键字参数有：
          av: 合并文件的时候调用 ffmpeg_concat_av 来合并视频片段 （download_urls）
          
          part:视频片段编号 （download_urls->get_output_filename）
          
          m3u8/m3u8_url:
          
          m3u8_type:     
          
          flush:
          
          file:
          
          post_data_raw:
          
          repo_path:



# 主要功能实现  ： extractor.py

这里有主要定义个类 VideoExtractor, 外部实现的下载接口可以继承这个类

### 一些类中主要的 变量：
 
**streams={}**:
  
  主要提供下载视频片段的url等信息。这个变量由外部需要由外部接口来进行初始化，结构如下：
  
  { 
  
    '1':   {'src':['url1','url2','url3',.....] ,  'size':0 ,  'container': 'MP4' ,  'video_profile':'1280x720_2000kb/s' ,'quality':'xxx', 'm3u8_url':'xxx'} , 
  
    '2':   {'src':['url1','url2','url3',.....] ,  'size':0 ,  'container': 'MP4' ,  'video_profile':'1280x720_1200kb/s' ,'quality':'xxx', 'm3u8_url':'xxx'} ,
   
    '3':   {'src':['url1','url2','url3',.....] ,  'size':0 ,  'container': 'MP4' ,  'video_profile':'640x360_850kb/s'  ,'quality':'xxx', 'm3u8_url':'xxx'  }  ,
    
    ...
 }
   
 
 **streams_sorted=[]** :   
 
 由streams 变换而来 =>  streams['1'].items()==>
 
>[('src', ['url1', 'url2', 'url3...']), ('size', 0), ('container', 'MP4'), ('video_profile', '1280x720_2000kb/s','quality':'xxx', 'm3u8_url':'xxx')]
 
 ==> [('id', '1')] + list(streams['1'].items())
 
>[ ('id', '1'), ('src', ['url1', 'url2', 'url3', ... ]), ('size', 0), ('container', 'MP4'), ('video_profile', '1280x720_1200kb/s','quality':'xxx', 'm3u8_url':'xxx')  ]
 
 ==> dict()
 
 >{'id': '1', 'src': ['url1', 'url2', 'url3'], 'size': 0, 'container': 'MP4', 'video_profile': '1280x720_2000kb/s','quality':'xxx', 'm3u8_url':'xxx'},
 
 >{'id': '2', 'src': ['url1', 'url2', 'url3'], 'size': 0, 'container': 'MP4', 'video_profile': '1280x720_1200kb/s','quality':'xxx', 'm3u8_url':'xxx'},
 
 >{'id': '3', 'src': ['url1', 'url2', 'url3'], 'size': 0, 'container': 'MP4', 'video_profile': '640x360_850kb/s','quality':'xxx', 'm3u8_url':'xxx'},
 
 ==> [ dict() ] 
 
 **streams_sorted** =
 
    [

    {'id': '1', 'src': ['url1', 'url2', 'url3'], 'size': 0, 'container': 'MP4', 'video_profile': '1280x720_2000kb/s','quality':'xxx', 'm3u8_url':'xxx'},
 
    {'id': '2', 'src': ['url1', 'url2', 'url3'], 'size': 0, 'container': 'MP4', 'video_profile': '1280x720_1200kb/s','quality':'xxx', 'm3u8_url':'xxx'},
 
    {'id': '3', 'src': ['url1', 'url2', 'url3'], 'size': 0, 'container': 'MP4', 'video_profile': '640x360_850kb/s','quality':'xxx', 'm3u8_url':'xxx'},
    
    ...

    ]
 
 **dash_streams={}**

 
### 一些类方法：

####download_by_vid(self, vid,**kwargs):

这里主要实现调用外部网页模块接口，生成streams  ，以及 streams_sorted 

然后调用 下面的  download(self, **kwargs）

#####download(**kwargs)

kwargs 关键字参数，可能包含 ：

    json_output :                             
    info_only :                 
    stream_id :                      
    index :                 
    output_dir :                       
    caption :                            
    output_dir :                             
    keep_obj :
                              
####download(self, **kwargs):



####p(self, stream_id=None):



###p_stream(self, stream_id):
打印streams 里面key 和value 值

# 扩展网站需要实现的接口
## download(url, **kwargs)
  
  
  
### download_playlist(url, **kwargs)
  

          file:
          
          post_data_raw:
          
          repo_path:



# 主要功能实现  ： extractor.py

这里有主要定义个类 VideoExtractor, 外部实现的下载接口可以继承这个类

### 一些类中主要的 变量：
 
 streams={} :
  
  主要提供下载视频片段的url等信息。这个变量由外部需要由外部接口来进行初始化，结构如下：
  
  { 
  
    '1':   {'src':['url1','url2','url3',.....] ,  'size':0 ,  'container': 'MP4' ,  'video_profile':'1280x720_2000kb/s' } , 
  
    '2':   {'src':['url1','url2','url3',.....] ,  'size':0 ,  'container': 'MP4' ,  'video_profile':'1280x720_1200kb/s' } ,
   
    '3':   {'src':['url1','url2','url3',.....] ,  'size':0 ,  'container': 'MP4' ,  'video_profile':'640x360_850kb/s'   }  ,
    
    ...
 }
   
 
 streams_sorted=[]  :   
 
 由streams 变换而来 =>  streams['1'].items()==>
 
>[('src', ['url1', 'url2', 'url3...']), ('size', 0), ('container', 'MP4'), ('video_profile', '1280x720_2000kb/s')]
 
 ==> [('id', '1')] + list(streams['1'].items())
 
 [ ('id', '1'), ('src', ['url1', 'url2', 'url3', ... ]), ('size', 0), ('container', 'MP4'), ('video_profile', '1280x720_1200kb/s')  ]
 
 ==> dict()
 
 {'id': '1', 'src': ['url1', 'url2', 'url3'], 'size': 0, 'container': 'MP4', 'video_profile': '1280x720_2000kb/s'},
 
 {'id': '2', 'src': ['url1', 'url2', 'url3'], 'size': 0, 'container': 'MP4', 'video_profile': '1280x720_1200kb/s'},
 
 {'id': '3', 'src': ['url1', 'url2', 'url3'], 'size': 0, 'container': 'MP4', 'video_profile': '640x360_850kb/s'},
 
 ==> [ dict() ] 
 
 **streams_sorted** =
 
 [
 
   {'id': '1', 'src': ['url1', 'url2', 'url3'], 'size': 0, 'container': 'MP4', 'video_profile': '1280x720_2000kb/s'},
 
   {'id': '2', 'src': ['url1', 'url2', 'url3'], 'size': 0, 'container': 'MP4', 'video_profile': '1280x720_1200kb/s'},
 
   {'id': '3', 'src': ['url1', 'url2', 'url3'], 'size': 0, 'container': 'MP4', 'video_profile': '640x360_850kb/s'},
   ...
 
 ]
 
 dash_streams={}
 
 
### 一些类方法：

download_by_vid(self, vid, **kwargs)

这里主要实现调用外部网页模块接口，生成streams  ，以及 streams_sorted 

然后调用 下面的  download(self, **kwargs）




download(self, **kwargs)

   kwargs 关键字参数，可能包含 ：
                              json_output
                              
                              info_only
                              
                              stream_id
                              
                              index
                              
                              output_dir
                              
                              caption
                              
                              output_dir
                              
                              keep_obj
                              
   

  
  # 扩展网站需要实现的接口
  ## download(url, **kwargs)
  
  
  
  ### download_playlist(url, **kwargs)
  
