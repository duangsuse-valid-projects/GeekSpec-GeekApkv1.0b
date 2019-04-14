# GeekApk: GeekSpec 示例定义文件 ![version] ![author] ![date]

> 这是由 [duangsuse](https://github.com/duangsuse) 创建的 GeekSpec Json Xhr API 接口定义语言描述的 GeekApk 接口和逻辑定义，GeekSpec __0.2__ 版本，使用 [GeekSpec 0.1](https://github.com/duangsuse/GeekApk/blob/master/geekspec_dsl_parser.pegjs) 的原文档可以在[这里](https://github.com/duangsuse/GeekApk/blob/master/geekapk_v1b_api.geekspec)查看

[version]: https://img.shields.io/badge/spec--version-1.0b-orange.svg?style=flat-square
[author]: https://img.shields.io/badge/author-duangsuse-brightgreen.svg?style=flat-square
[date]: https://img.shields.io/badge/date-Sun_Apr_14_2019_CST-9cf.svg?style=flat-square

## _GeekSpec_ for: __geekapk__, version: __0.2__, spec-version-short: __1.0b__, lang: __zh_CN__

## _Transmission :: *_ 数据交流格式

- scheme
  - https
- content-type
  - application/json; charset=utf-8

## _Description_ 应用程序描述

> GeekApk 的 GeekSpec 规范即将准备制定 v0.2 新版本，新的规范使用了宏预处理和 Markdown，将支持使用 SPEC 语言进行控制器编程（所以可以将能自动生成的逻辑尽可能自动生成）

所以就有你们现在看到的东西 — 不觉得它非常好看吗？ 😆

GeekSpec v0.2 会让 GeekApk 服务更开放，更优雅的呈现在大家眼前，它使得 JavaEE/其他 Web 后端架构的应用程序变得容易开发，接口逻辑皆可使用简洁的 GeekSpec 宏描述

GeekSpec v1.0 希望能对 Session, RDBMS Dao, State(Transaction), MVC, Endpoint binding, Dataverify, REST(Application path, Resource representation, HTTP verb, Forms, Path param, Default value), Middleware(WebFilter), Tests, Authentication / OAuth, UI 的打稿和 WebSockets 都能有内部支持，可是，GeekSpec v0.2 目前计划支持这些

以下的文字都是很菜很 ~~操🥚~~ 的我 ~~苦🍺~~ 快上学时（高二放假才放<abbr title="名词；白天的一半">半天</abbr>... 心累）抽出时间写的，可能很不成熟，各位大佬不喜勿喷

### GeekSpec，GeekApk 描述后端模型和逻辑算法的选择

#### GeekSpec 是什么，它和 GeekApk 又有什么关系？

GeekSpec, 全称 GeekApk's Specification Language, 一门用于描述现代简单 Http XmlHttpRequest + Json Web 接口的定义语言

在开发 [GeekApk SpringBoot](https://github.com/duangsuse/GeekApk) 和类似的 RDBMS + Json Http 后端服务器的过程中，为了进行最基础的 HTTP API 路由绑定，必须编写这样的控制器路径绑定代码（对于 Java EE 的应用服务器、[Servlet](https://blog.yuuta.moe/2017/05/30/webservlet-jetty-appserver/) 架构、[Vert.x Http Server](https://github.com/duangsuse/RandomPicture/blob/master/src/main/kotlin/org/duangsuse/fushion/RandomPicture.kt#L119) 也是一样），对于模型、业务需求比较大，接口比较多的应用程序，进行难看冗长的接口定义成为了应用开发过程中一件令人难受却又不得不做的码农级工作

```kotlin
@PutMapping("/{aid}")
@ResponseBody
fun updateApp(@PathVariable("aid") aid: AppId, @RequestParam("attr") attr: String/* Maybe package or icon or name or screenshots or readme */,
            @RequestBody value: String): Map<String, String> /* attr: String *//* oldVal: String */ {
TODO()
}
```

GeekApk 使用了一种独特的方式解决 endpoint 接口定义繁复的问题 — 制作了一个类似 Swagger 的工具帮助完成 Web 接口方面的工作，它是一门 DSL，被称为 GeekSpec

```plain
PUT@updateApp(aid-path:AppId, attr:String{package, icon, name, screenshots, readme}, val-body:String)
  -> [$attr:String, $oldVal:String]
  = /app/{aid}
```

GeekSpec __v0.1__ 使用类似下面的语法描述 Json HTTP 接口，允许 C/S 架构的客户端和服务端共享定义，解决了接口设置可维护性差的问题（即使 SpringBoot 使用 Annotation 自动设置路由，依然有代码重复冗长的问题），同时也使得接口代码更易读写

> 🤔 GeekSpec 对 GeekApk Spring 的工程有多大的辅助作用呢？自 [Spec 文件写完](https://github.com/duangsuse/GeekApk/commit/fb0242a475e84fe12911840de5d7740b8fd075e0)之后，自动生成解决了 300 多行模板代码的编写任务，从写完 7:34 到完成任务合并主分支 8:08 只花费了半个小时左右，可谓是一瞬生成了（ [telegram](https://t.me/dsuse/9050)

```plain
serverDescription() -> plain
  = /serverDescription

serverBoot() -> datetime
  = /serverBoot

PUT@resetSharedHash(uid-path:UserId, shash:String?) -> plain
= /admin/resetMetaHash/{uid}

PUT@resetHash(id-path:UserId, shash:String, hash:String)
  -> [$id:number, $newShash:string, $newHash:string]
  = /user/{id}/hash
```

duangsuse 为 GeekSpec 0.1 编写了四个工具

- [DSL Parser](https://github.com/duangsuse/GeekApk/blob/master/geekspec_dsl_parser.pegjs) 用于封装 PEG.js 描述的解析器，将 Spec 语言抽象树转化成 JSON 的格式输出
- [Spectrum](https://github.com/duangsuse/GeekApk/blob/master/spectrum.rb) 自动化 Ruby API 客户端
- [Spring Codegen](https://github.com/duangsuse/GeekApk/blob/master/code_writer.js) SpringBoot Endpoint binding Kotlin 代码生成工具
- [Doc to Spec](https://github.com/duangsuse/GeekApk/blob/master/translate_doc_geekspec.groovy) 早期 GeekSpec 还是一种伪代码描述语言时，使用的将 ApiDoc.kt 字符串常量转换到 GeekSpec 语言的翻译器

#### GeekSpec __<sup>v0.2</sup>__，新时代的关系型模型 Web 服务应用程序数据模型、接口、逻辑定义语言

GeekSpec 0.1 接口定义语言可以做很多事情，这包括

- 为东西命名
  - 接口名、参数名、实体类型名、返回、可选值的可选字符为 `[A-Za-z0-9_\-:]`
  - Dict 字段名的可选字符比上面的少一个 `:`
  - URL 的可选字符比上面的多 `[:/.{}?&=%]`
- 使用类型标记
  - 可能是 atom 的 `array` 或者 (value 为 atom 类型的) `object`，可是实现时 `object` 的语义混淆了
  - atom 可能是
    - boolean
    - number
    - string
    - datetime
    - plain
    - entity
- 定义一个 HTTP 接口
  - 定义默认 GET 的 HTTP 动词：GET, POST, PUT, PATCH, DELETE, OPTIONS, HEAD
    - `POST@createNewPost`
  - 定义接口的名称，诸如 `getServerVersion`，可能被作为生成代码的函数 / 方法名
  - 定义接口的参数列表
    - 有一个参数名，在 Kotlin Spring Codegen 的扩展里，后缀 `path` 来表示参数位置在路径 URL Template 里，后缀 `body` 表示此参数代表请求主体
    - 可能是可选参数，在名称后加 `?` 以实现
    - 可能是枚举参数，在参数后加 `{caseA,caseB,caseC}` 以实现
    - `PUT@updateReversion(aid-path:AppId, rev-path:Int, attr:String{version, install, updates, minsdk}, val-body:String)`
  - 定义接口的 Http 返回报文，这可以是
    - 一个 JSON 值
    - 一个被详细定义的 JSON 对象，包含所包含的键和键的类型，这被称为 Dict
      - `[$aid:number, $old:number, $new:number]`
      - `[$aid:number, $rev:number]`
  - 定义接口的 URL 路径，支持 URL Template
    - `readReversions(aid-path:AppId) -> array:AppUpdate = /appUpdate/{aid}`
- 使用 `#` 起始的行注释，忽视空白字符 `[ \t\n\r]*`，以换行符 `[\n\r\u2028\u2029]` 切分定义项

但是，它也有许多事情无法帮助我们完成，工具自己也有很多不完善的地方

- 设计时没有考虑如何和开发平台（比如 Java）、构建系统进行集成，仅仅考虑了进行手动代码生成
- 设计时没有考虑如何尽可能重用接口定义，导致定义本身用途有限、移植较为困难
- 没有支持 MultiPart Post
- 不支持权限验证
- 对 JAX-RS 和 HTTP 动词不是完整支持
- 不支持 REST Controller，参数和返回只能在非常狭隘的结构段上出现
- 不支持数据格式定义（比如特殊的 `datetime` 格式），无法定义转化器
- 类型定义方式不够优雅，意义不明确，扩展性不够，不能定义自己的类型（数据格式）
- 定义文件本身没有内建实现模块化，一旦定义项数目破百很难维护定义文件
- 没有内联文档支持，没有块注释
- 没有提供数据模型建模支持
- 无法解决线性业务逻辑自动模板化生成的问题
- 没有提供错误处理、HTTP 返回模式识别功能

而且，它的语法虽然简洁，但是不直观，如果用类似 Markdown 这样简洁直白的标记语言编写定义，是不是就同时解决了不直观、难加注释和需要额外编写文档生成器的问题呢？

于是，GeekSpec 的新版本 — __GeekSpec 0.2__ 被设计出来，目标在于完美解决 GeekApk v1.0ᵦ 的编写问题

## _Models_ 关系数据模型 ⭐️

_See_ [model definition]

[model definition]: geekapk-1.0beta-zh_CN/model.api.md

## _Interfaces_ 操作接口 ⭐️

_See_ [api definition]

[api definition]: geekapk-1.0beta-zh_CN/api.api.md

## _Authentication_ 身份验证 ⭐️️

_See_ [authentication definition]

[authentication definition]: geekapk-1.0beta-zh_CN/auth.api.md

## _Errors_ 接口错误 ⭐️

_See_ [api error definition]

[api error definition]: geekapk-1.0beta-zh_CN/error.api.md

## _Service_ GeekApk 服务器

- [GeekApk.org 官方服务 - v2]

[GeekApk.org 官方服务 - v2]: https://api.geekapk.org/v2/

## _Developers_ 开发者联系方式

- duangsuse _|_ 动苏
  - name: duangsuse
  - email: fedora-opensuse@outlook.com
  - github _|_ telegram: @duangsuse

## _License_ 许可证

本文档使用 __GNU Affero General Public License version 3 or later__ [license](https://www.gnu.org/licenses/agpl-3.0-standalone.html)

```plain
GeekApk API Specification version 0.1 beta
Copyright(C) 2019 duangsuse

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU Affero General Public License as published
by the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU Affero General Public License for more details.

You should have received a copy of the GNU Affero General Public License
along with this program.  If not, see <http://www.gnu.org/licenses/>.
```

<dl id="License_QA">
  <dt>我对源代码克隆的权利和义务
  <dd>自己 Google

  <dt>为啥不用 Creative Commons
  <dd>我不想用，<ruby>见仁见智<rt>毫无意义</rt></ruby>的问题...

  <dt>我才不遵守许可证
  <dd>随你便，反正这玩意也没花我几小时时间...
</dl>

## _See Also_ 引用链接

- [Rebase API Markdown](https://github.com/duangsuse/RebaseD/blob/master/rebase-api.md) drakeet 给他的 Rebase API 写的文档
- [Rebase API Swagger](https://app.swaggerhub.com/apis/duangsuse/RebaseApi/0.7.2) duangsuse 给官方文档写的 Swagger 定义
- [GeekApk API v0.1b](https://github.com/duangsuse/GeekApk/blob/master/geekapk_v1b_api.geekspec) 基于 GeekSpec v0.1 的 GeekApk API 0.1b
