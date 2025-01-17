FROM nvidia/cuda:11.3.0-devel-centos8 as base

FROM base as base-amd64

ENV NV_CUDNN_VERSION 8.2.0.53-1
ENV NV_CUDNN_PACKAGE libcudnn8-devel-${NV_CUDNN_VERSION}.cuda11.3

FROM base as base-arm64

ENV NV_CUDNN_VERSION 8.2.0.53-1
ENV NV_CUDNN_PACKAGE libcudnn8-devel-${NV_CUDNN_VERSION}.cuda11.3

FROM base as base-ppc64le

ENV NV_CUDNN_VERSION 8.2.0.53-1
ENV NV_CUDNN_PACKAGE libcudnn8-devel-${NV_CUDNN_VERSION}.cuda11.3

LABEL maintainer "NVIDIA CORPORATION <sw-cuda-installer@nvidia.com>"
LABEL com.nvidia.cudnn.version="${NV_CUDNN_VERSION}"

RUN dnf -y --disablerepo '*' --enablerepo=extras swap centos-linux-repos centos-stream-repos \
    && dnf distro-sync \
    && yum update -y

RUN yum install -y \
    ${NV_CUDNN_PACKAGE} \
    && yum clean all \
    && rm -rf /var/cache/yum/*

COPY install/centos_build.sh /install/centos_build.sh
RUN bash /install/centos_build.sh

COPY install/centos_conda.sh /install/centos_conda.sh
RUN bash /install/centos_conda.sh

COPY install/centos_dgl.sh /install/centos_dgl.sh
RUN bash /install/centos_dgl.sh

# COPY install/conda_env/tensorflow_gpu.yml /install/conda_env/tensorflow_gpu.yml
# RUN ["/bin/bash", "-i", "-c", "conda env create -f /install/conda_env/tensorflow_gpu.yml"]

# COPY install/conda_env/mxnet_gpu.yml /install/conda_env/mxnet_gpu.yml
# RUN ["/bin/bash", "-i", "-c", "conda env create -f /install/conda_env/mxnet_gpu.yml"]

# Environment variables
ENV PATH=/usr/local/nvidia/bin:${PATH}
ENV PATH=/usr/local/cuda/bin:${PATH}
ENV CPLUS_INCLUDE_PATH=/usr/local/cuda/include:${CPLUS_INCLUDE_PATH}
ENV C_INCLUDE_PATH=/usr/local/cuda/include:${C_INCLUDE_PATH}
ENV LIBRARY_PATH=/usr/local/cuda/lib64:/usr/local/nvidia/lib64:${LIBRARY_PATH}
ENV LD_LIBRARY_PATH=/usr/local/cuda/lib64:/usr/local/nvidia/lib64:${LD_LIBRARY_PATH}
ENV CUDA_VISIBLE_DEVICES=0,1,2,3
ENV TF_FORCE_GPU_ALLOW_GROWTH=true