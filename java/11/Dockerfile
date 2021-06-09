FROM openjdk:11-jdk

RUN wget -c https://johnvansickle.com/ffmpeg/builds/ffmpeg-git-amd64-static.tar.xz \
  && mkdir ffmpeg && tar xvJf ffmpeg-git-amd64-static.tar.xz -C ffmpeg --strip-components 1 \
  && install -m 0755 ffmpeg/ffmpeg /usr/bin/ && install -m 0755 ffmpeg/ffprobe /usr/bin/ \
  && rm -rf ffmpeg-git-amd64-static.tar.xz ffmpeg
