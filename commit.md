# 实现自定义 Git 提交规范

## 一、全局安装

- **全局依赖**

  ```sh
  npm install -g commitizen cz-conventional-changelog
  ```

- **查看安装依赖是否成功**

  ```sh
  npm list -g -depth=0
  ```

- 生成 **~/.czrc** 文件

  ```sh
  echo '{"path":"cz-conventional-changelog"}' > ~/.czrc
  ```

> 注意全局安装完成以后，在每次 **git commit** 时，就可以使用 **git cz** 了

## 二、项目依赖安装

- **安装依赖**

```sh
npm install -D commitizen cz-conventional-changelog
# 生成 .czrc 文件 
echo '{"path":"cz-conventional-changelog"}' > .czrc
```

> 注意：需要去掉 .czrc 文件中的 **单引号**

- 对 package.json 文件添加配置

  ```javascript
  "scripts":{
      commit:"git-cz"
  },
  "config":{
      "commitizen":{
          "path":"node_modules/cz-conventional-changelog"
      }
  }
  ```

  > 在 **git commit** 时，就可以使用 **git cz** 了

- 使用 **`Husky`** 生成 **`Git Hooks`**

- > husky 负责提供更易用的 git hook。 结合 git hook 来检验 commit message，这样当你的提交不符合规范时就会阻止你提交

  ```sh
  npm install -D husky
  ```

  > **`Husky6.0`** 版本不在配置 package.json 文件，而是使用命令生成 .husky 文件夹

  1. 初始化 husky

  ```sh
  npx husky-init
  ```

  会在当前目录下生成 .husky & pre-commit 文件，同时 package.json 会增加：

  ```sh
  "scripts": {
      "prepare": "husky install"
  }
  ```

