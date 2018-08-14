## 为什么使用VideoToolBox
之前使用ffmpeg来进行h264的解码与播放，发现会遇见一个很难重现的crash的问题，一直找不到解决方案，于是去看ffmpeg的一些底层的代。发现,其实ffmpeg硬解码也是用的iOS的VideoToolBox来进行解码。既然，ffmpeg是在VideoToolBox上来进行解码与播放，为什么不自己直接使用VideoToolBox来进行视频解码与播放。而且这样ipa的包的体积也要小很多。不用基于第三方框架，对于项目的控制力也更强。于是花了一个星期的时间研究VideoToolBox。
<br>
ps：在这里我使用的WebSocket是SocketRocket的第三方库，大家可以自己Google了解。

## 目录
### [了解H264](https://yoffieyf.github.io/Yoffie/flask/flask01)
<br>
### [使用VideoToolBox来进行解码](https://yoffieyf.github.io/Yoffie/flask/flask01)
<br>
### [播放视频](https://yoffieyf.github.io/Yoffie/flask/flask01)