**原文地址：https://steel-metacarpal-c53.notion.site/screenshot-to-code-7e8ec64dd0c0478f84901aefc3405979**


项目地址， https://github.com/abi/screenshot-to-code

如果你没有安装过Python或者Yarn，那就用下面两条命令来安装Python，Node或者Yarn

```jsx
brew install python
brew install node
brew install yarn
brew install git
```

并且通过一下两个命令来确认，安装是否成功

```jsx
node --version
npm --version
python --version
yarn --version

Node: v18.12.1
npm: 8.19.2
Python: 3.11.5
Yarn: 1.22.19
```

这个软件对版本要求并不高，所以最新版的就行，我用的版本如下，你可以对照一下

然后Clone这个软件包

```jsx
git clone https://github.com/abi/screenshot-to-code
# 进入软件目录
cd screenshot-to-code
# 进入后台目录
cd backend
# GPT4 的API key
echo "OPENAI_API_KEY=sk-your-key" > .env
# 安装Poetry 依赖包管理器
pip install poetry
# 安装依赖包
poetry install
# 激活命令行
poetry shell
# 运行程序
poetry run uvicorn main:app --reload --port 7000
```

后台运行好之后，再打开另外一个命令行

来运行前段程序

```jsx
# 同样的进入软件目录
cd screenshot-to-code
# 进入前台目录
cd frontend
# 安装前台依赖包
yarn
yarn dev
```

打开浏览器地址，就可以使用了

http://localhost:5173/