# my-webpack5-ts-react

webpack5+ts+react 的项目搭建

## 1. .gitignore 文件

vscode 使用 gitignore 插件 ctrl+shift+p 召唤命令面板，输入 Add gitignore 命令，即可在输入框输入系统或编辑器名字，来自动添加需要忽略的文件或文件夹至 .gitignore 中。

## 添加 Macos、Windows、Vscode 等

## 2. npmrc 文件

```
# 创建 .npmrc 文件
touch .npmrc
# 在该文件内输入配置
registry=https://registry.npm.taobao.org/
```

---

## 3.多人开发时代码格式的统一性

项目根目录下的 .editorconfig 文件

```
root = true

[*]
indent_style = space
indent_size = 2
charset = utf-8
trim_trailing_whitespace = true
insert_final_newline = true
end_of_line = lf

[*.md]
trim_trailing_whitespace = false
```

- indent_style ：缩进风格，可选配置有 tab  和 space 。
- indent_size ：缩进大小，可设定为 1-8  的数字，比如设定为 2 ，那就是缩进 2  个空格。
- charset ：编码格式，通常都是选 utf-8 。
- trim_trailing_whitespace ：去除多余的空格，比如你不小心在尾巴多打了个空格，它会给你自动去掉。
- insert_final_newline ：在尾部插入一行，个人很喜欢这个风格，当最后一行代码很长的时候，你又想对该行代码比较靠后的位置编辑时，不要太好用哦，建议大家也开上。
- end_of_line ：换行符，可选配置有 lf ，cr ，crlf ，会有三种的原因是因为各个操作系统之间的换行符不一致，这里有历史原因，有兴趣的同学自行了解吧，许多有名的开源库都是使用 lf ，我们姑且也跟跟风吧。

---

## 4.统一项目编程风格

> Prettier 拥有更多配置项（实际上也不多，数了下二十个），且能在发布流程中执行命令自动格式化，能够有效的使项目代码风格趋于统一。

```
npm install prettier -D
```

安装成功之后在根目录新建文件 .prettierrc ，输入以下配置：

```
{
  "trailingComma": "all",
  "tabWidth": 2,
  "semi": false,
  "singleQuote": true,
  "endOfLine": "lf",
  "printWidth": 120,
  "bracketSpacing": true,
  "arrowParens": "always"
}
```

- trailingComma ：对象的最后一个属性末尾也会添加 , ，比如 { a: 1, b: 2 }  会格式为 { a: 1, b: 2, } 。
- tabWidth ：缩进大小。
- semi ：分号是否添加，我以前从 C++转前端的，有一段时间非常不能忍受不加分号的行为，现在香的一匹。
- singleQuote ：是否单引号，绝壁选择单引号啊，不会真有人还用双引号吧？不会吧！😏
- jsxSingleQuote ：jsx 语法下是否单引号，同上。
- endOfLine ：与 .editorconfig  保持一致设置。
- printWidth ：单行代码最长字符长度，超过之后会自动格式化换行。
- bracketSpacing ：在对象中的括号之间打印空格， {a: 5}  格式化为 { a: 5 } 。
- arrowParens ：箭头函数的参数无论有几个，都要括号包裹。比如 (a) => {} ，如果设为 avoid ，会自动格式化为 a => {} 。

### 怎么实现自动格式化

> 当我们编辑完代码之后，按下 ctrl+s 保存就给我们自动把当前文件代码格式化了，既能实时查看格式化后的代码风格，又省去了命令执行代码格式化的多余工作

1. 先安装扩展 Prettier - Code formatter
2. vscode 根目录添加配置文件

```
mkdir .vscode
cd .vscode
touch settings.json
```

```
{
  // 指定哪些文件不参与搜索
  "search.exclude": {
    "**/node_modules": true,
    "dist": true,
    "yarn.lock": true
  },
  "editor.formatOnSave": true,
  "[javascript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[javascriptreact]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[typescript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[typescriptreact]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[json]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[html]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[markdown]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[css]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[less]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[scss]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  }
}
```

- "editor.formatOnSave"  的作用是在我们保存时，会自动执行一次代码格式化，而我们该使用什么格式化器？接下来的代码便是设置默认的格式化器，看名字大家也能看得出来了吧！

- 在遇到 .js 、 .jsx 、.ts 、.tsx 、.json 、.html 、.md 、 .css 、 .less 、 .scss  为后缀的文件时，都会去使用 Prettier  去格式化代码，而格式化的规则就是我们配置的 .prettierrc  决定的！

* .editorconfig 配置文件中某些配置项是会和 Prettier 重合的，例如 指定缩进大小 两者都可以配置。

  1.  而 Prettier  是一个格式化工具，要根据具体语法格式化，对于不同的语法用单引号还是双引号，加不加分号，哪里换行等，当然，肯定也有缩进大小。即使缩进大小这些共同都有的设置，两者也是不冲突的，设置 EditorConfig  的 indent_size  为 4 ， Prettier  的 tabWidth  为 2 。

  2.  但是保存时，因为我们默认的格式化工具已经在 .vscode/settings.json 中设置为了 Prettier ，所以这时候读取缩进大小为 2 的配置，并正确格式化了代码

  3.  建议大家两个都配置文件重合的地方都保持一致比较好

---

## 4. ESLint

在上面我们配置了 EditorConfig  和 Prettier  都是为了解决代码风格问题，而 ESLint  是主要为了解决代码质量问题，它能在我们编写代码时就检测出程序可能出现的隐性 BUG，通过 eslint --fix  还能自动修复一些代码写法问题，比如你定义了 var a = 3 ，自动修复后为 const a = 3 。还有许多类似的强制扭转代码最佳写法的规则，在无法自动修复时，会给出红线提示，强迫开发人员为其寻求更好的解决方案。

> prettier 代码风格统一支持的语言更多，而且差异化小，eslint 一大堆的配置能弄出一堆风格，prettier 能对 ts js html css json md 做风格统一，这方面 eslint 比不过。

```
 npm install eslint -D

 npx eslint --init
```

关于 npx 的讲解 ---> https://www.yuque.com/frontend-onhmj/mt58ci/nbgvd1

在漫长的安装结束后，项目根目录下多出了新的文件 .eslintrc.js

```
module.exports = {
  env: {
    browser: true,
    es2020: true,
    node: true,
  },
  extends: ['plugin:react/recommended', 'airbnb'],
  parser: '@typescript-eslint/parser',
  parserOptions: {
    ecmaFeatures: {
      jsx: true,
    },
    ecmaVersion: 11,
    sourceType: 'module',
  },
  plugins: ['react', '@typescript-eslint'],
  rules: {},
}
```

各个属性字段的作用可在 Configuring ESLint 仔细了解,可能会比较迷惑的地方是 extends 和 plugins 的关系，其实 plugins 就是插件的意思，都是需要 npm 包的安装才可以使用，只不过默认支持简写，官网都有说；至于 extneds 其实就是使用我们已经下载的插件的某些预设规则。

- 根据 eslint-config-airbnb 官方说明，如果要开启 React Hooks 的检查，需要在 extends 中添加一项 'airbnb/hooks' 。
- 根据 @typescript-eslint/eslint-plugin 官方说明，在 extends 中添加 'plugin:@typescript-eslint/recommended'  可开启针对 ts 语法推荐的规则定义。
- 需要在.eslint.js 添加一条很重要的 rule ，不然在 .ts 和 .tsx 文件中引入另一个文件模块会报错 ：

```
rules: {
  'import/extensions': [
    ERROR,
    'ignorePackages',
    {
      ts: 'never',
      tsx: 'never',
      json: 'never',
      js: 'never',
    },
  ],
}
```

- 后续如果安装 typscript 的，可以先添加如下配置，把最常用的扩展名排在最前面，这里寻找文件时最快能找到

```
 settings: {
    'import/resolver': {
      node: {
        extensions: ['.tsx', '.ts', '.js', '.json'],
      },
    },
  },
```

接下来安装 2 个社区中比较火的 eslint  插件：

```
eslint-plugin-promise ：让你把 Promise 语法写成最佳实践。
eslint-plugin-unicorn ：提供了更多有用的配置项，比如我会用来规范关于文件命名的方式。
```

执行以下命令进行安装：

```
npm install eslint-plugin-promise eslint-plugin-unicorn -D
```

在添加了部分规则 rules  后，我的配置文件修改之后如下：

```
const OFF = 0
const WARN = 1
const ERROR = 2

module.exports = {
  env: {
    browser: true,
    es2020: true,
    node: true,
  },
  extends: [
    'airbnb',
    'airbnb/hooks',
    'plugin:react/recommended',
    'plugin:unicorn/recommended',
    'plugin:promise/recommended',
    'plugin:@typescript-eslint/recommended',
  ],
  parser: '@typescript-eslint/parser',
  parserOptions: {
    ecmaFeatures: {
      jsx: true,
    },
    ecmaVersion: 11,
    sourceType: 'module',
  },
  settings: {
    'import/resolver': {
      node: {
        extensions: ['.tsx', '.ts', '.js', '.json'],
      },
    },
  },
  plugins: ['react', 'unicorn', 'promise', '@typescript-eslint'],
  rules: {
    // 具体添加的其他规则大家可查看我的 github 查看
    // https://github.com/vortesnail/react-ts-quick-starter/blob/master/.eslintrc.js
  },
}

```

### ESLint 和 Prettier 的冲突

但根据官方推荐配置方法，既需要下载 eslint-config-prettier 也需要下载 eslint-plugin-prettier，然后配置更简洁了：

```
npm install --save-dev eslint-config-prettier
npm install --save-dev eslint-plugin-prettier
```

```
{
  extends: [
    // other extends ...
    'plugin:prettier/recommended',
  ],
  plugins: [
    // other plugins,
    'prettier'
  ],
}
```

---

## 5. StyleLint

> js 或 ts 代码已经能有良好的代码风格了，但可别忘了还有样式代码的风格也需要统一

```
npm install stylelint stylelint-config-standard -D
```

然后在项目根目录新建 .stylelintrc.js 文件，输入以下内容：

```
module.exports = {
  extends: ['stylelint-config-standard'],
  rules: {
    'comment-empty-line-before': null,
    'declaration-empty-line-before': null,
    'function-name-case': 'lower',
    'no-descending-specificity': null,
    'no-invalid-double-slash-comments': null,
    'rule-empty-line-before': 'always',
  },
  ignoreFiles: ['node_modules/**/*', 'build/**/*'],
}
```

- extends ：其实和 eslint  的类似，都是扩展，使用 stylelint  已经预设好的一些规则。
- rules ：就是具体的规则，如果默认的你不满意，可以自己决定某个规则的具体形式。
- ignoreFiles ：不像 eslint  需要新建 ignore 文件， stylelint  配置就支持忽略配置字段，我们先添加 node_modules  和 build ，之后有需要大家可自行添加。

  > 其中关于 xxx/\*_/_ 这种写法的意思有不理解的，大家可在 google （或百度）glob 模式。
