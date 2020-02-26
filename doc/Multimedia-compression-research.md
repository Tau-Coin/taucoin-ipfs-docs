### Multimedia compression research
在Android手机多媒体开发中，随着手机硬件设备性能的提高，多媒体的文件的大小也显著提高，特别是视频文件。由于论坛业务需求可以使用的媒体包括文字、图片、音频、视频，就要面临图片、音频、视频上传流量和存储较大的问题，解决方案为对多媒体数据压缩处理。以下默认以小米9手机为例：

#### 1、 Image的压缩

Android中常见的Image的类型有png、jpeg、jpg、gif、bmp、webp等，默认格式为jpg

常见的压缩方式为质量压缩和尺寸压缩；这里采用对不同尺寸的图片进行质量压缩测试：以jpg格式为例

<table>
  <tr>
    <th>压缩前大小(kb)</th>
    <th>尺寸(px)</th>
    <th>压缩后大小(kb)</th>
    <th>清晰度</th>
  </tr>
  <tr>
    <td>4732</td>
    <td>4000 x 3000</td>
    <td>267</td>
    <td>清晰</td>
  </tr>
  <tr>
    <td>4732</td>
    <td>4000 x 3000</td>
    <td>200</td>
    <td>清晰</td>
  </tr>
  <tr>
    <td>4732</td>
    <td>4000 x 3000</td>
    <td>74</td>
    <td>清晰—小部分失真</td>
  </tr>
  <tr>
    <td>4732</td>
    <td>4000 x 3000</td>
    <td>47</td>
    <td>大部分失真</td>
  </tr>
</table>

<table>
  <tr>
    <th>压缩前大小(kb)</th>
    <th>尺寸</th>
    <th>压缩后大小(kb)</th>
    <th>清晰度</th>
  </tr>
  <tr>
    <td>207</td>
    <td>1079 x 894 </td>
    <td>32</td>
    <td>清晰</td>
  </tr>
  <tr>
   <td>207</td>
    <td>1079 x 894 </td>
    <td>23</td>
    <td>清晰</td>
  </tr>
  <tr>
   	 <td>207</td>
    <td>1079 x 894 </td>
    <td>13</td>
    <td>清晰—小部分失真</td>
  </tr>
  <tr>
    <td>207</td>
    <td>1079 x 894 </td>
    <td>10</td>
    <td>大部分失真</td>
  </tr>
</table>

根据测试数据结果：<br/>
1、不考虑文件尺寸的话，建议大小限制在120kb左右；<br/>
2、考虑图片大小限制建议1200 x 1200，大小限制在100kb

#### 2、 Audio的压缩
Audio的类型有mp3、amr、aac、war、flac、lamr等：默认格式为mp3。

##### 专业名词说明
##### 采样率
采样率：采样率即采样频率，指每秒钟取得声音样本的次数，采样频率越高，能表现的频率范围就越大，音质就会越好，声音的还原度也更真实，但此同时带来的弊端是占有的内存资源也会越大。因为人耳的分辨率有限，并不是频率越高越好，44KHz已相当于CD音质了，目前的常用采样频率都不超过48KHz。

##### 声道
声道：这个好理解，生活中也经常听到单声道、双声道等，在Android系统中，可以通过设置音频的录制的声道 CHANNEL\_IN\_STEREO 为双声道，CHANNEL\_CONFIGURATION\_MONO 为单声道，双声道音质更加，但同样伴随着内存资源消耗更大的弊端。

##### 采样位深
采样位深：位深度也叫采样位深，音频的位深度决定动态范围，它是用来衡量声音波动变化的一个参数，也可以说是声卡的分辨率。它的数值越大，分辨率也就越高，所发出声音的能力越强。在计算机中采样位数一般有8位和16位之分，即分成2的8次方和2的16次方之分，PCM 16位每个样本，保证设备支持。PCM 8位每个样本，不一定能得到设备支持。

##### 压缩测试数据（mp3）：
手机(小米9)录音1分钟，文件大小为739kb;<br/>
手机(小米9)录音20分钟，文件大小为15257.6kb;

8,000 Hz - 电话所用采样率, 对于人的说话已经足够<br/>
11,025 Hz-AM调幅广播所用采样率<br/>
22,050 Hz和24,000 Hz- FM调频广播所用采样率<br/>
32,000 Hz - miniDV 数码视频 camcorder、DAT (LP mode)所用采样率<br/>
44,100 Hz - 音频 CD, 也常用于 MPEG-1 音频（VCD, SVCD, MP3）所用采样率<br/>
47,250 Hz - 商用 PCM 录音机所用采样率<br/>
48,000 Hz - miniDV、数字电视、DVD、DAT、电影和专业音频所用的数字声音所用采样率<br/>
50,000 Hz - 商用数字录音机所用采样率<br/>

采用音频处理框架[FFmpeg-Android-Java](https://github.com/WritingMinds/ffmpeg-android-java)对音频压缩，截取操作：<br/>
"-ab", bitrate<br/>
"-ar", freq<br/>
"-ac", channels<br/>
<table>
  <tr>
    <th>时长</th>
    <th>压缩前大小(kb)</th>
    <th>采样率(KHz)</th>
    <th>压缩后大小(kb)</th>
    <th>输出格式</th>
    <th>压缩占比（%）</th>
    <th>压缩后大小/秒（kb）</th>
  </tr>
  <tr>
  	 <td>00:05:14</td>
  	 <td>12851.2</td>
    <td>44.1—CD</td>
    <td>2570.24</td>
    <td>mp3</td>
    <td>20.0</td>
    <td>8.19</td>
  </tr>
  <tr>
  	 <td>00:05:14</td>
  	 <td>12851.2</td>
  	 <td>22.05—FM</td>
    <td>1290.24</td>
    <td>mp3</td>
    <td>10.0</td>
    <td>4.11</td>
  </tr>
  <tr>
  	 <td>00:05:14</td>
  	 <td>12851.2</td>
  	 <td>11.025—AM</td>
    <td>627.92</td>
    <td>mp3</td>
    <td>4.9</td>
    <td>1.98</td>
  </tr>
  <tr>
  	 <td>00:05:14</td>
  	 <td>12851.2</td>
  	 <td>8—Phone</td>
    <td>314.18</td>
    <td>mp3</td>
    <td>2.4</td>
    <td>1.00</td>
  </tr>
 </table>
根据测试数据结果建议：<br/>
1、如果音频文件大小小于1Mb不压缩处理，直接跳过；<br/>
2、大于1Mb,判断音频时长是否大于10分钟；先截取前10分钟，按电话的采样率再进行压缩；

#### 3、Video的压缩
Video的类型有mp4、avi、3gpp、3gp、mov等：默认格式为mp4。<br/>

Video在实际论坛开发中，采用从Video截取9张图片和一个语音以文件夹的方式存储到IPFS上，然后拿到文件夹的CID上链实现；

采用框架[FFmpeg-Android-Java](https://github.com/WritingMinds/ffmpeg-android-java)对视频提取图片的参数：<br/>
"-ss", "00:01:00",	// 开始时间<br/>
"-t", "00:09:00",	// 总共多长时间<br/>
"-vf fps=0.016",		// 帧率<br/>
"-q:v", "1",			// 质量<br/>

采用框架[FFmpeg-Android-Java](https://github.com/WritingMinds/ffmpeg-android-java)对视频提取音频的参数：<br/>
"-vn",<br/>
"-ss", "00:00:00",	// 开始时间<br/>
"-t", "00:10:00", 	// 总共多长时间<br/>
"-ab", "8", 			// 比特率<br/>
"-ar", "8000",  		// 采样率<br/>
"-ac", "1",   		// 声道数<br/>

### 总结：<br/>
1、多媒体的大小和其质量息息相关；<br/>
2、音频和视频的大小和其时长也密不可分；<br/>
3、压缩多媒体的过程必须要考虑用户体验问题；<br/>
4、综合考虑多媒体处理相关配置只能达到一种平衡来解决。
