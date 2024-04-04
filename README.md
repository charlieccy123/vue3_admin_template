# Vue 3 + TypeScript + Vite

This template should help get you started developing with Vue 3 and TypeScript in Vite. The template uses Vue 3 `<script setup>` SFCs, check out the [script setup docs](https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup) to learn more.

## 初始化项目

```shell
npm i -g pnpm

pnpm create vite
✔ Project name: … project
✔ Select a framework: › Vue
✔ Select a variant: › TypeScript

cd PROJECT_NAME && pnpm install
```

## 安装的插件

### eslint 配置

```shell
cd PROJECT_NAME
npx eslint --init

You can also run this command directly using 'npm init @eslint/config'.
Need to install the following packages:
@eslint/create-config@0.4.6
Ok to proceed? (y) y
✔ How would you like to use ESLint? · problems
✔ What type of modules does your project use? · esm
✔ Which framework does your project use? · vue
✔ Does your project use TypeScript? · No / Yes
✔ Where does your code run? · browser
✔ What format do you want your config file to be in? · JavaScript
The config that you've selected requires the following dependencies:

@typescript-eslint/eslint-plugin@latest eslint-plugin-vue@latest @typescript-eslint/parser@latest
✔ Would you like to install them now? · No / Yes
✔ Which package manager do you want to use? · pnpm
```

#### vue3环境代码校验插件

`eslint-config-prettier`,`eslint-plugin-import`,`eslint-plugin-node`让所有与prettier规则存在冲突的eslint rules失效，并使用
prettier进行代码检查

`eslint-plugin-prettier`运行更漂亮的eslint，使prettier规则优先级更高，eslint优先级低

`@babel/eslint-parser`该解析器允许使用eslint校验所有babel code

```shell
pnpm install -D eslint-plugin-import eslint-plugin-vue eslint-plugin-node eslint-plugin-prettier eslint-config-prettier \
eslint-plugin-node @babel/eslint-parser
```

修改`.eslintrc.cjs`配置文件

```js
module.exports = {
  // 运行环境
  env: {
    browser: true, // 浏览器
    es2021: true,
    node: true,
    jest: true,
  },
  // 规则继承
  extends: [
    // 全部规则默认是开启的，这个配置项开启推荐规则
    'eslint:recommended',
    // ts语法规则
    'plugin:@typescript-eslint/recommended',
    // vue3语法规则
    'plugin:vue/vue3-essential',
    'plugin:prettier/recommended',
  ],
  // 要为特定类型的文件指定处理器
  overrides: [
    {
      env: {
        node: true,
      },
      files: ['.eslintrc.{js,cjs}'],
      parserOptions: {
        sourceType: 'script',
      },
    },
  ],
  /* 指定如何解析语法 */
  parser: 'vue-eslint-parser',
  // 指定解析器
  parserOptions: {
    ecmaVersion: 'latest', // 校验ecma最新版本
    parser: '@typescript-eslint/parser', // ts解析器
    sourceType: 'module',
    jsxPragma: 'React',
    ecmaFeatures: {
      jsx: true,
    },
  },
  // eslint 支持使用的第三方插件，使用之前需要npm进行安装
  // 该eslint-plugin- 前缀可以从插件名称中省略，如下，完整的应该是 eslint-plugin-vue
  plugins: [
    '@typescript-eslint', // 检测ts语法的插件
    'vue', // 检测vue语法的插件
  ],
  // eslint 规则
  /*
   * "off" 或 0    ==>  关闭规则
   * "warn" 或 1   ==>  打开的规则作为警告（不影响代码执行）
   * "error" 或 2  ==>  规则作为一个错误（代码不能执行，界面报错）
   */
  rules: {
    // eslint（https://eslint.bootcss.com/docs/rules/）
    'no-var': 'error', // 要求使用 let 或 const 而不是 var
    'no-multiple-empty-lines': ['warn', { max: 1 }], // 不允许多个空行
    'no-console': process.env.NODE_ENV === 'production' ? 'error' : 'off',
    'no-debugger': process.env.NODE_ENV === 'production' ? 'error' : 'off',
    'no-unexpected-multiline': 'error', // 禁止空余的多行
    'no-useless-escape': 'off', // 禁止不必要的转义字符

    // typeScript (https://typescript-eslint.io/rules)
    '@typescript-eslint/no-unused-vars': 'error', // 禁止定义未使用的变量
    '@typescript-eslint/prefer-ts-expect-error': 'error', // 禁止使用 @ts-ignore
    '@typescript-eslint/no-explicit-any': 'off', // 禁止使用 any 类型
    '@typescript-eslint/no-non-null-assertion': 'off',
    '@typescript-eslint/no-namespace': 'off', // 禁止使用自定义 TypeScript 模块和命名空间。
    '@typescript-eslint/semi': 'off',

    // eslint-plugin-vue (https://eslint.vuejs.org/rules/)
    'vue/multi-word-component-names': 'off', // 要求组件名称始终为 “-” 链接的单词
    'vue/script-setup-uses-vars': 'error', // 防止<script setup>使用的变量<template>被标记为未使用
    'vue/no-mutating-props': 'off', // 不允许组件 prop的改变
    'vue/attribute-hyphenation': 'off', // 对模板中的自定义组件强制执行属性命名样式
  },
}
```

配置好后需要修改`package.json`来设置使用eslint

```json
{
  "scripts": {
    "dev": "vite",
    "build": "vue-tsc && vite build",
    "preview": "vite preview",
    "lint": "eslint src", // 新增检查指令
    "fix": "eslint src --fix" // 新增修复指令
  }
}
```

### 配置prettier

eslint是针对js或者ts或者vue的语法检测工具，而prettier是格式化工具

```shell
pnpm install -D eslint-plugin-prettier prettier eslint-config-prettier
```

添加规则，创建`.prettierrc.json`文件

```js
{
  "singleQuote": true,
  "semi": false,
  "bracketSpacing": true,
  "htmlWhitespaceSensitivity": "ignore",
  "endOfLine": "auto",
  "trailingComma": "all",
  "tabWidth": 2
}
```

创建忽略文件`.prettierignore`

```ignore
/dist/*
/html/*
.local
/node_modules/**
**/*.svg
**/*.sh
/public/*
```

如何使用呢？`pnpm run lint`用于检查，`pnpm un fix`用于修复

### 配置stylelint

他是css的lint工具，格式化css代码，检查css语法错误

```shell
pnpm add sass sass-loader stylelint postcss postcss-scss postcss-html \
stylelint-config-prettier stylelint-config-recess-order stylelint-config-recommended-scss \
stylelint-config-standard stylelint-config-standard-vue stylelint-scss stylelint-order \
stylelint-config-standard-scss -D
```

创建`.stylelintrc.cjs`

```js
// @see https://stylelint.bootcss.com/

module.exports = {
  extends: [
    'stylelint-config-standard', // 配置stylelint拓展插件
    'stylelint-config-html/vue', // 配置 vue 中 template 样式格式化
    'stylelint-config-standard-scss', // 配置stylelint scss插件
    'stylelint-config-recommended-vue/scss', // 配置 vue 中 scss 样式格式化
    'stylelint-config-recess-order', // 配置stylelint css属性书写顺序插件,
    'stylelint-config-prettier', // 配置stylelint和prettier兼容
  ],
  overrides: [
    {
      files: ['**/*.(scss|css|vue|html)'],
      customSyntax: 'postcss-scss',
    },
    {
      files: ['**/*.(html|vue)'],
      customSyntax: 'postcss-html',
    },
  ],
  ignoreFiles: [
    '**/*.js',
    '**/*.jsx',
    '**/*.tsx',
    '**/*.ts',
    '**/*.json',
    '**/*.md',
    '**/*.yaml',
  ],
  /**
   * null  => 关闭该规则
   * always => 必须
   */
  rules: {
    'value-keyword-case': null, // 在 css 中使用 v-bind，不报错
    'no-descending-specificity': null, // 禁止在具有较高优先级的选择器后出现被其覆盖的较低优先级的选择器
    'function-url-quotes': 'always', // 要求或禁止 URL 的引号 "always(必须加上引号)"|"never(没有引号)"
    'no-empty-source': null, // 关闭禁止空源码
    'selector-class-pattern': null, // 关闭强制选择器类名的格式
    'property-no-unknown': null, // 禁止未知的属性(true 为不允许)
    'block-opening-brace-space-before': 'always', //大括号之前必须有一个空格或不能有空白符
    'value-no-vendor-prefix': null, // 关闭 属性值前缀 --webkit-box
    'property-no-vendor-prefix': null, // 关闭 属性前缀 -webkit-mask
    'selector-pseudo-class-no-unknown': [
      // 不允许未知的选择器
      true,
      {
        ignorePseudoClasses: ['global', 'v-deep', 'deep'], // 忽略属性，修改element默认样式的时候能使用到
      },
    ],
  },
}
```

创建忽略文件`.stylelintignore`

```ignore
/node_modules/*
/dist/*
/html/*
/public/*
```

配置统一的prettier来格式化js和css以及html代码，在package.json的scripts里添加语句`"format": "prettier --write \"./**/*.{html,vue,ts,js,json,md}\"",`

### 配置husky

配置好代码检查工具，但是需要每次手动去执行才会进行格式化，如果有人没有格式化就提交远程仓库，那这个规范就没有用了，所以我们要前置开发按照规范来提交。
我们就利用husky在代码提交前触发git hook（git 在用户本地的钩子），然后执行`pnpm run format`来自动执行格式化代码。

首先，安装`pnpm install husky -D`

其次初始化目录`git init`，然后关联远程仓库并推送代码，然后执行`npx husky-init`，他会在根目录生产一个`.husky`目录，在目录下会有个`pre-commit`文件，文件里面的命令在我们执行`git commit`
的时候会执行.

在`.husky/pre-commit`文件添加如下命令：

```shell
#!/usr/bin/env sh
. "$(dirname -- "$0")/_/huksy.sh"
pnpm run format
```

### 配置commitlint

这个是对commit信息做规范的工具，不能随便写，要让每个人按照统一的标准来执行。

安装`pnpm add @commitlint/config-conventional @commitlint/cli -D`

根目录添加配置文件`commitlint.config.cjs`

```js
module.exports = {
  ignores: [(commit) => commit.includes('init')],
  extends: ['@commitlint/config-conventional'],
  rules: {
    'type-enum': [
      2,
      'always',
      [
        'feat',
        'fix',
        'docs',
        'style',
        'refactor',
        'perf',
        'test',
        'chore',
        'revert',
        'build',
      ],
    ],
    'type-case': [0],
    'type-empty': [0],
    'scope-empty': [0],
    'scope-case': [0],
    'subject-full-stop': [0, 'never'],
    'subject-case': [0, 'never'],
    'header-max-length': [0, 'always', 72],
  },
}
```

然后在`package.json`中配置`scripts`命令

```js
{
  "scripts": {
    "commitlint": "commitlint --config commitlint.config.cjs -e -V"
  }
}
```

配置结束后，当我们填写commit信息的时候，前面需要带下面的subject

feat 新特性，新功能
fix 修改bug
docs 文档修改
style 代码格式修改，注意这里不是css修改
refactor 代码重构
pref 优化相关，比如提升性能或者体验
test 测试用例修改
chore 其他修改，比如改变构建流程或者增加依赖库、工具等
revert 会滚到上一个版本
build 编译相关修改，例如发布版本，对项目构建或者依赖的改动

配置husky`npx husky add .husky/commit-msg`，他会在`.husky`目录里生成`commit-msg`文件，然后添加下面的内容

```shell
#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"

pnpm commitlint
```

这样当我们commit信息的时候，就不能随便写里，必须是`git commit -m "fix: xxxxxx"`，需要注意的是`fix: `必须有冒号，冒号后面必须有空格。
