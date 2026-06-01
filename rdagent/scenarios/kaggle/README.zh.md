# Kaggle 爬虫

## 在 Linux 上安装 Chrome 和 chromedriver

在同一文件夹中执行：

```shell
# 安装 chrome
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
sudo apt install ./google-chrome-stable_current_amd64.deb
google-chrome --version

# 安装 chromedriver
wget https://storage.googleapis.com/chrome-for-testing-public/<chrome-version>/linux64/chromedriver-linux64.zip
unzip chromedriver-linux64.zip
cd chromedriver-linux64
sudo mv chromedriver /usr/local/bin
sudo chmod +x /usr/local/bin/chromedriver

chromedriver --version
```

## 配置

1. 认证：`~/.kaggle/kaggle.json`
2. 在竞赛网站接受规则（加入竞赛）

## Notebook 爬虫

1. `download_notebooks()` - 下载 notebooks
2. `convert_notebooks_to_text()` - 转换为文本