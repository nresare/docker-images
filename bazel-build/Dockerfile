FROM amazonlinux:2.0.20220606.1 as builder

RUN yum -y install git gcc-c++ golang which unzip
# java-1.8.0-openjdk-devel
#ADD jdk-17.0.4+8 /jdk-17.0.4+8

RUN go install github.com/bazelbuild/bazelisk@latest
ENV PATH $PATH:$HOME/go/bin
# bazel binary built from the add_remotejdk17_linux_aarch64 branch of https://github.com/nresare/bazel
#ADD bazel /usr/bin/bazel
RUN git clone --depth 1 https://github.com/nresare/bazel --branch add_remotejdk17_linux_aarch64
RUN yum -y install python3
RUN (cd bazel && $HOME/go/bin/bazelisk build  --java_runtime_version=remotejdk_11 //src:bazel)


FROM amazonlinux:2.0.20220606.1
COPY --from=builder /bazel/bazel-bin/src/bazel /usr/bin/bazel
COPY --from=builder /usr/bin/git /usr/bin/git
COPY --from=builder /usr/share/git-core /usr/share/git-core
RUN yum -y install git gcc-c++ java-1.8.0-openjdk-headless
RUN git clone https://github.com/nresare/nryaml
RUN (cd nryaml && bazel test //...)
RUN rm -rf nryaml
