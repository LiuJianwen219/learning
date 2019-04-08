## 一、Python 环境

项目运行选用 Python3.4 。树莓派3自带 Python3.4 环境，无需安装。

### pip

若系统未提供 pip 工具，可通过以下指令安装。

```
$ wget https://bootstrap.pypa.io/get-pip.py
$ python3 get-pip.py
```

### virtualenv

```
$ sudo pip install virtualenv
```

## 二、 FFmpeg

### libfdk-aac

```plain
$ sudo apt-get install pkg-config autoconf automake libtool
$ git clone https://github.com/mstorsjo/fdk-aac.git
$ cd fdk-aac
$ ./autogen.sh
$ ./configure --enable-shared --enable-static
$ make
$ sudo make install
$ sudo ldconfig
```

### 其他依赖

```
$ sudo apt-get install libopus-dev libopenjpeg-dev libmp3lame-dev libpulse-dev libv4l-dev libvorbis-dev
```

### 编译安装FFmpeg

```
$ git clone https://github.com/FFmpeg/FFmpeg.git --depth=1
$ cd FFmpeg
$ ./configure \
    --prefix=/usr \
    --disable-debug \
    --disable-static \
    --enable-gpl \
    --enable-nonfree \
    --enable-libfdk-aac \
    --enable-libmp3lame \
    --enable-libopenjpeg \
    --enable-libopus \
    --enable-libpulse \
    --disable-librtmp \
    --enable-libv4l2 \
    --enable-libvorbis \
    --enable-runtime-cpudetect \
    --enable-shared \
    --enable-version3 \
    --disable-x11grab \
    --disable-doc \
    --disable-dxva2 \
    --disable-vaapi \
    --disable-vdpau \
    --disable-decoder=aac_latm \
    --disable-decoder=aasc \
    --disable-decoder=adpcm_4xm \
    --disable-decoder=adpcm_adx \
    --disable-decoder=adpcm_afc \
    --disable-decoder=adpcm_ct \
    --disable-decoder=adpcm_ea \
    --disable-decoder=adpcm_ea_maxis_xa \
    --disable-decoder=adpcm_ea_r1 \
    --disable-decoder=adpcm_ea_r2 \
    --disable-decoder=adpcm_ea_r3 \
    --disable-decoder=adpcm_ea_xas \
    --disable-decoder=adpcm_g722 \
    --disable-decoder=adpcm_g726 \
    --disable-decoder=adpcm_ima_amv \
    --disable-decoder=adpcm_ima_apc \
    --disable-decoder=adpcm_ima_dk3 \
    --disable-decoder=adpcm_ima_dk4 \
    --disable-decoder=adpcm_ima_ea_eacs \
    --disable-decoder=adpcm_ima_ea_sead \
    --disable-decoder=adpcm_ima_iss \
    --disable-decoder=adpcm_ima_oki \
    --disable-decoder=adpcm_ima_qt \
    --disable-decoder=adpcm_ima_smjpeg \
    --disable-decoder=adpcm_ima_wav \
    --disable-decoder=adpcm_ima_ws \
    --disable-decoder=adpcm_ms \
    --disable-decoder=adpcm_sbpro_2 \
    --disable-decoder=adpcm_sbpro_3 \
    --disable-decoder=adpcm_sbpro_4 \
    --disable-decoder=adpcm_swf \
    --disable-decoder=adpcm_thp \
    --disable-decoder=adpcm_xa \
    --disable-decoder=adpcm_yamaha \
    --disable-decoder=alac \
    --disable-decoder=als \
    --disable-decoder=amrnb \
    --disable-decoder=amrwb \
    --disable-decoder=amv \
    --disable-decoder=anm \
    --disable-decoder=ansi \
    --disable-decoder=ape \
    --disable-decoder=ass \
    --disable-decoder=asv1 \
    --disable-decoder=asv2 \
    --disable-decoder=atrac1 \
    --disable-decoder=atrac3 \
    --disable-decoder=aura \
    --disable-decoder=aura2 \
    --disable-decoder=avrn \
    --disable-decoder=avrp \
    --disable-decoder=avs \
    --disable-decoder=avui \
    --disable-decoder=bethsoftvid \
    --disable-decoder=bfi \
    --disable-decoder=bink \
    --disable-decoder=binkaudio_dct \
    --disable-decoder=binkaudio_rdft \
    --disable-decoder=bintext \
    --disable-decoder=bmv_audio \
    --disable-decoder=bmv_video \
    --disable-decoder=brender_pix \
    --disable-decoder=c93 \
    --disable-decoder=cavs \
    --disable-decoder=cdgraphics \
    --disable-decoder=cdxl \
    --disable-decoder=cinepak \
    --disable-decoder=cljr \
    --disable-decoder=cllc \
    --disable-decoder=comfortnoise \
    --disable-decoder=cook \
    --disable-decoder=cpia \
    --disable-decoder=cscd \
    --disable-decoder=cyuv \
    --disable-decoder=dca \
    --disable-decoder=dfa \
    --disable-decoder=dirac \
    --disable-decoder=dnxhd \
    --disable-decoder=dpx \
    --disable-decoder=dsicinaudio \
    --disable-decoder=dsicinvideo \
    --disable-decoder=dvbsub \
    --disable-decoder=dvvideo \
    --disable-decoder=dxa \
    --disable-decoder=dxtory \
    --disable-decoder=eacmv \
    --disable-decoder=eamad \
    --disable-decoder=eatgq \
    --disable-decoder=eatgv \
    --disable-decoder=eatqi \
    --disable-decoder=eightbps \
    --disable-decoder=eightsvx_exp \
    --disable-decoder=eightsvx_fib \
    --disable-decoder=escape124 \
    --disable-decoder=escape130 \
    --disable-decoder=evrc \
    --disable-decoder=exr \
    --disable-decoder=flic \
    --disable-decoder=fourxm \
    --disable-decoder=fraps \
    --disable-decoder=frwu \
    --disable-decoder=gsm \
    --disable-decoder=gsm_ms \
    --disable-decoder=iac \
    --disable-decoder=idcin \
    --disable-decoder=idf \
    --disable-decoder=iff_byterun1 \
    --disable-decoder=iff_ilbm \
    --disable-decoder=imc \
    --disable-decoder=indeo2 \
    --disable-decoder=indeo3 \
    --disable-decoder=indeo4 \
    --disable-decoder=indeo5 \
    --disable-decoder=interplay_dpcm \
    --disable-decoder=interplay_video \
    --disable-decoder=jacosub \
    --disable-decoder=jpegls \
    --disable-decoder=jv \
    --disable-decoder=kgv1 \
    --disable-decoder=kmvc \
    --disable-decoder=lagarith \
    --disable-decoder=loco \
    --disable-decoder=mace3 \
    --disable-decoder=mace6 \
    --disable-decoder=mdec \
    --disable-decoder=microdvd \
    --disable-decoder=mimic \
    --disable-decoder=mjpegb \
    --disable-decoder=mlp \
    --disable-decoder=mmvideo \
    --disable-decoder=motionpixels \
    --disable-decoder=mp1 \
    --disable-decoder=mp1float \
    --disable-decoder=mp2float \
    --disable-decoder=mp3adu \
    --disable-decoder=mp3adufloat \
    --disable-decoder=mp3on4 \
    --disable-decoder=mp3on4float \
    --disable-decoder=mpl2 \
    --disable-decoder=msa1 \
    --disable-decoder=msrle \
    --disable-decoder=mss1 \
    --disable-decoder=mss2 \
    --disable-decoder=msvideo1 \
    --disable-decoder=mszh \
    --disable-decoder=mts2 \
    --disable-decoder=mvc1 \
    --disable-decoder=mvc2 \
    --disable-decoder=mxpeg \
    --disable-decoder=nuv \
    --disable-decoder=paf_audio \
    --disable-decoder=paf_video \
    --disable-decoder=pam \
    --disable-decoder=pbm \
    --disable-decoder=pcm_bluray \
    --disable-decoder=pcm_lxf \
    --disable-decoder=pcm_zork \
    --disable-decoder=pcx \
    --disable-decoder=pgssub \
    --disable-decoder=pictor \
    --disable-decoder=pjs \
    --disable-decoder=prores \
    --disable-decoder=prores_lgpl \
    --disable-decoder=ptx \
    --disable-decoder=qcelp \
    --disable-decoder=qdm2 \
    --disable-decoder=qdraw \
    --disable-decoder=qpeg \
    --disable-decoder=r10k \
    --disable-decoder=r210 \
    --disable-decoder=ra_144 \
    --disable-decoder=ra_288 \
    --disable-decoder=ralf \
    --disable-decoder=realtext \
    --disable-decoder=rl2 \
    --disable-decoder=roq \
    --disable-decoder=roq_dpcm \
    --disable-decoder=rpza \
    --disable-decoder=rv10 \
    --disable-decoder=rv20 \
    --disable-decoder=rv30 \
    --disable-decoder=rv40 \
    --disable-decoder=s302m \
    --disable-decoder=sami \
    --disable-decoder=sanm \
    --disable-decoder=sgi \
    --disable-decoder=sgirle \
    --disable-decoder=shorten \
    --disable-decoder=sipr \
    --disable-decoder=smackaud \
    --disable-decoder=smacker \
    --disable-decoder=smc \
    --disable-decoder=snow \
    --disable-decoder=sol_dpcm \
    --disable-decoder=sonic \
    --disable-decoder=sp5x \
    --disable-decoder=subviewer \
    --disable-decoder=subviewer1 \
    --disable-decoder=sunrast \
    --disable-decoder=svq1 \
    --disable-decoder=svq3 \
    --disable-decoder=tak \
    --disable-decoder=targa \
    --disable-decoder=targa_y216 \
    --disable-decoder=thp \
    --disable-decoder=tiertexseqvideo \
    --disable-decoder=tmv \
    --disable-decoder=truehd \
    --disable-decoder=truemotion1 \
    --disable-decoder=truemotion2 \
    --disable-decoder=truespeech \
    --disable-decoder=tscc \
    --disable-decoder=tscc2 \
    --disable-decoder=tta \
    --disable-decoder=twinvq \
    --disable-decoder=txd \
    --disable-decoder=ulti \
    --disable-decoder=utvideo \
    --disable-decoder=v210 \
    --disable-decoder=v210x \
    --disable-decoder=v308 \
    --disable-decoder=v408 \
    --disable-decoder=v410 \
    --disable-decoder=vb \
    --disable-decoder=vble \
    --disable-decoder=vc1 \
    --disable-decoder=vc1image \
    --disable-decoder=vcr1 \
    --disable-decoder=vima \
    --disable-decoder=vmdaudio \
    --disable-decoder=vmdvideo \
    --disable-decoder=vmnc \
    --disable-decoder=vplayer \
    --disable-decoder=vqa \
    --disable-decoder=webvtt \
    --disable-decoder=wmalossless \
    --disable-decoder=wmapro \
    --disable-decoder=wmav1 \
    --disable-decoder=wmav2 \
    --disable-decoder=wmavoice \
    --disable-decoder=wmv1 \
    --disable-decoder=wmv2 \
    --disable-decoder=wmv3 \
    --disable-decoder=wmv3image \
    --disable-decoder=wnv1 \
    --disable-decoder=ws_snd1 \
    --disable-decoder=xan_dpcm \
    --disable-decoder=xan_wc3 \
    --disable-decoder=xan_wc4 \
    --disable-decoder=xbin \
    --disable-decoder=xbm \
    --disable-decoder=xface \
    --disable-decoder=xl \
    --disable-decoder=xsub \
    --disable-decoder=xwd \
    --disable-decoder=yop \
    --disable-decoder=zero12v \
    --disable-decoder=zerocodec \
    --disable-decoder=zmbv \
    --disable-encoder=ayuv \
    --disable-encoder=a64multi \
    --disable-encoder=a64multi5 \
    --disable-encoder=aac \
    --disable-encoder=ac3 \
    --disable-encoder=ac3_fixed \
    --disable-encoder=adpcm_adx \
    --disable-encoder=adpcm_g722 \
    --disable-encoder=adpcm_g726 \
    --disable-encoder=adpcm_ima_qt \
    --disable-encoder=adpcm_ima_wav \
    --disable-encoder=adpcm_ms \
    --disable-encoder=adpcm_swf \
    --disable-encoder=adpcm_yamaha \
    --disable-encoder=alac \
    --disable-encoder=amv \
    --disable-encoder=ass \
    --disable-encoder=asv1 \
    --disable-encoder=asv2 \
    --disable-encoder=avrp \
    --disable-encoder=avui \
    --disable-encoder=cljr \
    --disable-encoder=comfortnoise \
    --disable-encoder=dca \
    --disable-encoder=dnxhd \
    --disable-encoder=dpx \
    --disable-encoder=dvbsub \
    --disable-encoder=dvdsub \
    --disable-encoder=dvvideo \
    --disable-encoder=eac3 \
    --disable-encoder=jpegls \
    --disable-encoder=msmpeg4v2 \
    --disable-encoder=msmpeg4v3 \
    --disable-encoder=msvideo1 \
    --disable-encoder=pam \
    --disable-encoder=pcx \
    --disable-encoder=prores \
    --disable-encoder=prores_anatoliy \
    --disable-encoder=prores_kostya \
    --disable-encoder=r10k \
    --disable-encoder=r210 \
    --disable-encoder=ra_144 \
    --disable-encoder=roq \
    --disable-encoder=roq_dpcm \
    --disable-encoder=rv10 \
    --disable-encoder=rv20 \
    --disable-encoder=sgi \
    --disable-encoder=snow \
    --disable-encoder=sonic \
    --disable-encoder=sonic_ls \
    --disable-encoder=sunrast \
    --disable-encoder=svq1 \
    --disable-encoder=targa \
    --disable-encoder=tiff \
    --disable-encoder=utvideo \
    --disable-encoder=v210 \
    --disable-encoder=v308 \
    --disable-encoder=v408 \
    --disable-encoder=v410 \
    --disable-encoder=wmav1 \
    --disable-encoder=wmav2 \
    --disable-encoder=wmv1 \
    --disable-encoder=wmv2 \
    --disable-encoder=xbm \
    --disable-encoder=xface \
    --disable-encoder=xsub \
    --disable-encoder=xwd \
    --disable-encoder=y41p \
    --disable-encoder=zmbv \
    --disable-muxer=a64 \
    --disable-muxer=adx \
    --disable-muxer=aiff \
    --disable-muxer=amr \
    --disable-muxer=asf \
    --disable-muxer=asf_stream \
    --disable-muxer=ass \
    --disable-muxer=ast \
    --disable-muxer=au \
    --disable-muxer=avm2 \
    --disable-muxer=bit \
    --disable-muxer=caf \
    --disable-muxer=cavsvideo \
    --disable-muxer=daud \
    --disable-muxer=dirac \
    --disable-muxer=dnxhd \
    --disable-muxer=dts \
    --disable-muxer=dv \
    --disable-muxer=eac3 \
    --disable-muxer=f4v \
    --disable-muxer=filmstrip \
    --disable-muxer=framecrc \
    --disable-muxer=framemd5 \
    --disable-muxer=gxf \
    --disable-muxer=ipod \
    --disable-muxer=ircam \
    --disable-muxer=ismv \
    --disable-muxer=ivf \
    --disable-muxer=jacosub \
    --disable-muxer=microdvd \
    --disable-muxer=mlp \
    --disable-muxer=mmf \
    --disable-muxer=mxf \
    --disable-muxer=mxf_d10 \
    --disable-muxer=oma \
    --disable-muxer=psp \
    --disable-muxer=rm \
    --disable-muxer=roq \
    --disable-muxer=rso \
    --disable-muxer=sox \
    --disable-muxer=swf \
    --disable-muxer=tg2 \
    --disable-muxer=tgp \
    --disable-muxer=truehd \
    --disable-muxer=vc1t \
    --disable-muxer=voc \
    --disable-muxer=w64 \
    --disable-muxer=wtv \
    --disable-muxer=wv \
    --disable-demuxer=act \
    --disable-demuxer=adf \
    --disable-demuxer=adx \
    --disable-demuxer=aea \
    --disable-demuxer=afc \
    --disable-demuxer=amr \
    --disable-demuxer=anm \
    --disable-demuxer=apc \
    --disable-demuxer=aqtitle \
    --disable-demuxer=asf \
    --disable-demuxer=ass \
    --disable-demuxer=ast \
    --disable-demuxer=au \
    --disable-demuxer=avr \
    --disable-demuxer=avs \
    --disable-demuxer=bethsoftvid \
    --disable-demuxer=bfi \
    --disable-demuxer=bink \
    --disable-demuxer=bit \
    --disable-demuxer=bmv \
    --disable-demuxer=brstm \
    --disable-demuxer=c93 \
    --disable-demuxer=caf \
    --disable-demuxer=cavsvideo \
    --disable-demuxer=cdg \
    --disable-demuxer=cdxl \
    --disable-demuxer=daud \
    --disable-demuxer=dfa \
    --disable-demuxer=dirac \
    --disable-demuxer=dnxhd \
    --disable-demuxer=dsicin \
    --disable-demuxer=dts \
    --disable-demuxer=dtshd \
    --disable-demuxer=dv \
    --disable-demuxer=dxa \
    --disable-demuxer=ea \
    --disable-demuxer=ea_cdata \
    --disable-demuxer=eac3 \
    --disable-demuxer=epaf \
    --disable-demuxer=filmstrip \
    --disable-demuxer=flic \
    --disable-demuxer=fourxm \
    --disable-demuxer=frm \
    --disable-demuxer=gsm \
    --disable-demuxer=gxf \
    --disable-demuxer=ico \
    --disable-demuxer=idcin \
    --disable-demuxer=idf \
    --disable-demuxer=iff \
    --disable-demuxer=ingenient \
    --disable-demuxer=ipmovie \
    --disable-demuxer=ircam \
    --disable-demuxer=iss \
    --disable-demuxer=iv8 \
    --disable-demuxer=ivf \
    --disable-demuxer=jacosub \
    --disable-demuxer=jv \
    --disable-demuxer=latm \
    --disable-demuxer=lmlm4 \
    --disable-demuxer=loas \
    --disable-demuxer=lvf \
    --disable-demuxer=lxf \
    --disable-demuxer=mgsts \
    --disable-demuxer=microdvd \
    --disable-demuxer=mlp \
    --disable-demuxer=mm \
    --disable-demuxer=mmf \
    --disable-demuxer=mpl2 \
    --disable-demuxer=mpsub \
    --disable-demuxer=msnwc_tcp \
    --disable-demuxer=mtv \
    --disable-demuxer=mv \
    --disable-demuxer=mvi \
    --disable-demuxer=mxf \
    --disable-demuxer=mxg \
    --disable-demuxer=nc \
    --disable-demuxer=nistsphere \
    --disable-demuxer=nsv \
    --disable-demuxer=nuv \
    --disable-demuxer=oma \
    --disable-demuxer=paf \
    --disable-demuxer=pjs \
    --disable-demuxer=pmp \
    --disable-demuxer=pva \
    --disable-demuxer=pvf \
    --disable-demuxer=qcp \
    --disable-demuxer=r3d \
    --disable-demuxer=realtext \
    --disable-demuxer=rl2 \
    --disable-demuxer=rm \
    --disable-demuxer=roq \
    --disable-demuxer=rpl \
    --disable-demuxer=rso \
    --disable-demuxer=sami \
    --disable-demuxer=sap \
    --disable-demuxer=sbg \
    --disable-demuxer=segafilm \
    --disable-demuxer=shorten \
    --disable-demuxer=siff \
    --disable-demuxer=smacker \
    --disable-demuxer=smjpeg \
    --disable-demuxer=smush \
    --disable-demuxer=sol \
    --disable-demuxer=sox \
    --disable-demuxer=spdif \
    --disable-demuxer=str \
    --disable-demuxer=subviewer \
    --disable-demuxer=subviewer1 \
    --disable-demuxer=swf \
    --disable-demuxer=tak \
    --disable-demuxer=tedcaptions \
    --disable-demuxer=thp \
    --disable-demuxer=tiertexseq \
    --disable-demuxer=tmv \
    --disable-demuxer=truehd \
    --disable-demuxer=tta \
    --disable-demuxer=txd \
    --disable-demuxer=vc1 \
    --disable-demuxer=vc1t \
    --disable-demuxer=vivo \
    --disable-demuxer=vmd \
    --disable-demuxer=vobsub \
    --disable-demuxer=voc \
    --disable-demuxer=vplayer \
    --disable-demuxer=vqf \
    --disable-demuxer=w64 \
    --disable-demuxer=wc3 \
    --disable-demuxer=webvtt \
    --disable-demuxer=wsaud \
    --disable-demuxer=wsvqa \
    --disable-demuxer=wtv \
    --disable-demuxer=wv \
    --disable-demuxer=xa \
    --disable-demuxer=xbin \
    --disable-demuxer=xmv \
    --disable-demuxer=xwma \
    --disable-demuxer=yop \
    --disable-protocol=crypto \
    --disable-protocol=gopher \
    --disable-parser=adx \
    --disable-parser=cavsvideo \
    --disable-parser=cook \
    --disable-parser=dca \
    --disable-parser=dirac \
    --disable-parser=dnxhd \
    --disable-parser=gsm \
    --disable-parser=mlp \
    --disable-parser=rv30 \
    --disable-parser=rv40 \
    --disable-parser=tak \
    --disable-parser=vc1 \
    --disable-indev=jack \
    --disable-indev=fbdev \
    --disable-outdev=xv \
    --disable-outdev=sdl
$ make
$ sudo make install
```

## 三、 syslog

新建 `/etc/rsyslog.d/exotic-rpi.conf` ，添加以下内容：

```
if $syslogfacility-text == 'local2' and $programname == 'exotic_rpi' then /var/log/exotic-rpi/syslog
```

然后重启 `rsyslog`。

```
$ sudo service rsyslog restart
```

## 四、 `gpio` utility

```
$ sudo apt-get install wiringpi
```

## 五、 生产环境

```
$ mkdir -p /var/www
$ cd /var/www
$ git clone https://github.com/All-less/exotic-rpi.git --depth=1
$ cd exotic-rpi
$ virtualenv env -p python3
$ env/bin/pip install -r requirements/common.txt
```

在工程的顶层目录下新建 `config.py`。典型配置文件如下：

```python
# -*- coding: utf-8 -*-

host = 'xxx.xxx.xxx.xxx'  # 服务器地址
port = xxxx               # 服务器端口

device_id = 'test'
auth_key = '7aeb64ae34a82383f46472ae46644bb4'

platform = 'sword'

deploy = 'PROD'
debug = False
```

通过以下命令启动：

```
$ sudo scripts/daemon.sh start
```

程序的日志位于 `/var/log/exotic-rpi/syslog` 。程序 `stdout` 会被重定向到 `/var/log/exotic-rpi/stdout.log` ， `stderr` 会被重定向到 `/var/log/exotic-rpi/stderr.log` 。

## 六、 开发环境

开发环境下对于项目位置没有要求。

```
$ git clone https://github.com/All-less/exotic-rpi.git
$ cd exotic-rpi
$ virtualenv env -p python3
$ env/bin/pip install -r requirements/common.txt
$ env/bin/pip install -r requirements/dev.txt
```

在工程的顶层目录下新建 `config.py`。典型配置文件如下：

```python
# -*- coding: utf-8 -*-

host = 'xxx.xxx.xxx.xxx'  # 调试服务器地址
port = xxxx               # 调试服务器端口

device_id = 'test'
auth_key = '7aeb64ae34a82383f46472ae46644bb4'

platform = 'sword'

deploy = 'DEV'
debug = True
```

通过以下命令启动程序：

```
./main.py --config=config.py
```

## 七、 远程控制、部署

在开发环境的工程目录下，可通过 [`Fabric`](fabfile.org) 控制远端树莓派上程序的启动、停止及自动更新。

执行以下命令即可启动和停止远端树莓派上的程序。其中 `<user>:<host>` 即为 SSH 所使用的信息。

```
$ env/bin/fab -H <user>@<host> start
$ env/bin/fab -H <user>@<host> stop
```

执行以下命令可控制树莓派自动从 git 拉取最近的代码并重启程序。

```
$ env/bin/fab -H <user>@<host> deploy
```
