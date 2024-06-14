FROM direct.virtaicloud.com:32402/njnonpry6rws/1-20240305142236:20240305142236
ENV LC_ALL=zh_CN.UTF-8

# 设置 pip 源并升级 pip
RUN pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple && \
    pip install --upgrade pip

# 强制重新安装 wrapt 包
RUN pip install --ignore-installed wrapt

# 安装其他所需的包
RUN pip install --no-cache-dir jupyterlab==3.2.9 && \
    pip install --no-cache-dir jupyterlab-language-pack-zh-CN && \
    pip install --no-cache-dir 'jupyterlab>=3.2.0,<5.0.0a0' jupyterlab-lsp && \
    pip install --no-cache-dir 'python-lsp-server[all]' && \
    ln -s / .lsp_symlink && \
    pip install --no-cache-dir jupyterlab-git && \
    pip install --no-cache-dir ipympl && \
    pip install --no-cache-dir jupyterlab-spreadsheet-editor && \
    pip install --no-cache-dir jupyter-resource-usage && \
    pip install --no-cache-dir lckr_jupyterlab_variableinspector && \
    pip install --no-cache-dir jupyterlab-tensorboard-pro

# 创建 Jupyter 工作目录
RUN mkdir -p /mnt/jupyter

ENTRYPOINT ["jupyter", "lab", "--allow-root", "--ip=0.0.0.0", "--notebook-dir=/mnt/jupyter", "--ServerApp.allow_origin=*", "--ServerApp.quit_button=False", "--ServerApp.disable_check_xsrf=True", "--ServerApp.allow_remote_access=True"]