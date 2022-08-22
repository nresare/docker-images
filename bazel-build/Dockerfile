FROM amazonlinux:2.0.20220606.1 as builder

RUN yum -y install git gcc-c++ golang which unzip python3
# java-1.8.0-openjdk-devel
#ADD jdk-17.0.4+8 /jdk-17.0.4+8

RUN go install github.com/bazelbuild/bazelisk@latest
ENV PATH $PATH:$HOME/go/bin

ARG TARGETARCH
RUN if [ "$TARGETARCH" = "arm64" ]; then \
 git clone --depth 1 https://github.com/nresare/bazel --branch add_remotejdk17_linux_aarch64 && \
 (cd bazel && $HOME/go/bin/bazelisk build  --java_runtime_version=remotejdk_11 //src:bazel) && \
 cp bazel/bazel-bin/src/bazel /bazel-binary; \
 else \
 cp $HOME/go/bin/bazelisk /bazel-binary; \
 fi
#RUN git clone https://github.com/nresare/nryaml
#RUN yum -y install java-1.8.0-openjdk-headless
#RUN (cd nryaml && /bazel-binary test //...)

FROM amazonlinux:2.0.20220606.1
RUN yum -y install git gcc-c++ java-1.8.0-openjdk-headless
COPY --from=builder /bazel-binary /usr/bin/bazel
#COPY --from=builder /root/.cache/bazel/_bazel_root/cache /root/.cache/bazel/_bazel_root/cache
