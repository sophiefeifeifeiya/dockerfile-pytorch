FROM nvcr.io/nvidia/pytorch:20.07-py3
ENV DEBIAN_FRONTEND=noninteractive

# 安装必要的包
RUN rm -rf /etc/apt/sources.list.d/cuda* /etc/apt/sources.list.d/* && \
    gpg --keyserver keyserver.ubuntu.com --recv-keys A4B469963BF863CC 2>&1 > /dev/null && \
    gpg --export --armor A4B469963BF863CC | apt-key add - 2>&1 > /dev/null && \
    apt-get update && \
    apt-get install -y tmux screen vim wget curl net-tools apt-utils unzip zip git openssl libaio1 libaio-dev iputils-ping openssh-server openssh-client && \
    apt-get install -y git-lfs libssl-dev openssl make gcc libffi-dev zlib1g-dev libbz2-dev software-properties-common nginx lsof iftop htop tcpdump sysstat && \
    apt-get install -y locate cmake g++ build-essential zlib1g-dev libncurses5-dev libgdbm-dev language-pack-zh-hans tree libgl1 libglib2.0-0 && \
    apt-get install -y libnss3-dev libssl-dev libreadline-dev libffi-dev libsqlite3-dev libbz2-dev libjpeg-dev && \
    apt-get install -y build-essential liblzma-dev libsqlite3-dev pkg-config libnuma-dev libgl1-mesa-glx xvfb && \
    apt-get install -y libssh2-1-dev ca-certificates libfreetype6-dev mlocate swig llvm libncursesw5-dev tk-dev && \
    apt-get install -y openmpi-bin openmpi-common libopenmpi-dev && apt-get clean && rm -rf /var/lib/apt/lists/*

# 安装 GitHub CLI
RUN curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | gpg --dearmor -o /usr/share/keyrings/githubcli-archive-keyring.gpg && \
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | tee /etc/apt/sources.list.d/github-cli.list > /dev/null && \
    apt update && apt install gh && apt-get clean && rm -rf /var/lib/apt/lists/* /etc/apt/sources.list.d/*

# 配置 pip 并安装 Python 包
RUN pip config set global.index-url https://pypi.org/simple && \
    pip install --upgrade pip && \
    pip config set global.timeout 60

RUN pip install --no-cache-dir seaborn rich mido matplotlib pandas seaborn keras-tuner pillow tqdm einops scikit-learn fluidsynth pretty-midi \
    einops pyfluidsynth wurlitzer pyglet pyvirtualdisplay pydot tensorboard jupyterlab==3.2.9 jupyterlab-language-pack-zh-CN

# 安装 PaddlePaddle GPU 版本
RUN pip install --no-cache-dir paddlepaddle-gpu==2.0.2.post110 -f https://paddlepaddle.org.cn/whl/mkl/stable.html

# 设置 CUDA 环境变量
ENV ORION_CUDART_VERSION=11.0

# 最终配置
RUN rm -rf /root/.config && pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple

# 创建 Jupyter 工作目录
RUN mkdir -p /mnt/jupyter

# 创建并配置启动脚本
RUN echo "#!/usr/bin/env bash" > /run_jupyter.sh && \
    echo "jupyter lab --allow-root --ip=0.0.0.0 --notebook-dir=/mnt/jupyter --ServerApp.allow_origin='*' --ServerApp.quit_button=False --ServerApp.disable_check_xsrf=True --ServerApp.allow_remote_access=True \"\$@\"" >> /run_jupyter.sh && \
    chmod +x /run_jupyter.sh

# 设置容器入口点
ENTRYPOINT ["/run_jupyter.sh"]