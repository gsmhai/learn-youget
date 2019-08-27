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

  kwargs： 关键字参数一般有：
          av: 合并文件的时候调用 ffmpeg_concat_av 来合并视频片段 （download_urls）
          part:视频片段编号 （download_urls->get_output_filename）
          m3u8/m3u8_url:
          m3u8_type:          
          flush:
          file:
          post_data_raw:
          repo_path:


  
  # 扩展网站需要实现的接口
  ## download(url, **kwargs)
  
  
  
  ### download_playlist(url, **kwargs)
  
