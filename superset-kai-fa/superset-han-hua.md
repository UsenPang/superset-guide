# Superset汉化

## 汉化配置

superset支持多种语言，其汉化文件在superset/translations，默认是英文，如果要使用中文可以在superset\_config.py文件中进行配置

<pre class="language-editorconfig"><code class="lang-editorconfig"><strong>#设置默认语言为中文
</strong><strong>BABEL_DEFAULT_LOCALE = "zh"
</strong>LANGUAGES = {
    "zh": {"flag": "cn", "name": "简体中文"},
    "en": {"flag": "us", "name": "English"},
}
</code></pre>

然后重启服务就完成了



## 自定义汉化

官方的汉化有很多单词是错的；并且随着项目的更新，有些单词汉化更不上更新的速度，甚至没有翻译。想要自己去汉化就必须知道superset是怎么实现汉化的。

#### **认识babel**

在superset的后端py代码中使用了Flask-Babel来翻译，Flask-Babel是一个为Flask框架提供国际化和本地化（i18n/l10n）支持的插件，它基于Babel库来实现文本的翻译、数字格式化、日期格式化等功能。

#### **使用babel**

superset后端使用`gettext` 和`lazy_gettext` (或者别名`_`）方法,在前端使用`t`和`tn`来进行翻译，如：gettext('translate me')、t('translate me')

使用翻译工具时先确认是否安装了依赖

```
pip install -r superset/translations/requirements.txt
```

#### **提取翻译字符串**

使用`gettext`、`t`等方法包含的字符串会被提取到.pot文件中，.pot文件是一个多语言的翻译模板文件。

提取字符串,这个脚本会提取前后端所有的翻译字符串：

```
./scripts/translations/babel_update.sh
```

#### **更新翻译文件**

后面我们需要根据.pot翻译模板文件生成新的.po语言文件：

```
 pybabel update -i superset/translations/messages.pot -d superset/translations --ignore-obsolete
```

然后就可以开始翻译了，翻译位于 superset/translation 下的po文件中收集的字符串，其中每种语言都有一个文件夹。

```
 pybabel update -i superset/translations/messages.pot -d superset/translations --ignore-obsolete
```

推荐一个翻译po文件的工具，可以更方便快捷的翻译：poedit。

#### **编译翻译文件**

前端：

<pre><code><strong>cd superset-frontend/ &#x26;&#x26; npm ci
</strong>
npm run build-translation
</code></pre>

上面的命令会生成一个messages.json的文件供前端使用

后端：

```
# inside the project root
pybabel compile -d superset/translations
```

上面的命令会生成一个messages.mo的文件供后端使用



