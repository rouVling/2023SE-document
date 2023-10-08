# 后端环境配置
以下内容和小作业部分文档相同，可以使用小作业环境

## 环境配置

我们使用 Linux（或 WSL）环境与 `Python=3.9` 配置本次作业，推荐你使用 `conda` 创建一个新的虚拟环境：

```bash
conda create -n django_hw python=3.9 -y
conda activate django_hw
```

在此环境的基础之上，你可以运行下述命令安装依赖，注意请确保你的当前工作路径在克隆的小作业仓库中：

```bash
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple -r requirements.txt
```

!!! note "配置环境也是软件工程的一部分"

    软件工程是一门研究用工程化方法构建和维护有效的、实用的和高质量的软件的学科，而配置环境是任何工程化项目的第一步。在本次作业中，我们使用了 `conda` 作为环境管理工具，使用了 `pip` 作为依赖管理工具。这些工具的使用都是为了让你能够更加方便地配置环境，从而更加专注于实现功能。在大作业中，你也会使用到类似的工具，因此请务必熟悉这些工具的使用方法。


然后，你可以运行如下指令检查环境配置是否成功：

```bash
python3 manage.py runserver
```

这会在 `localhost:8000` 开启服务端进行监听网络请求。你可以打开浏览器，访问 http://localhost:8000/startup 来检查服务端是否正常启动。如果正常启动，你会看到含有 "Congratulations! You have successfully installed the requirements. Go ahead!" 的网页。

pymongo的配置<br>
```
python -m pip install pymongo==4.5.0
```
