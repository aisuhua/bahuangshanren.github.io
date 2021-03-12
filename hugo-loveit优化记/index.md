# Hugo+loveit优化记


## LoveIt扩展Shortcodes

更多扩展Shortcodes的应用方法请查看LoveIt主题作者写的 [使用说明](https://hugoloveit.com/zh-cn/theme-documentation-extended-shortcodes/) 。

### admonition

admonition比较常用，有12个样式，但是主题作者并没说明每种样式对应的 `type` 是什么。我从源码中找到了它们的对应关系，在此记录一下。

#### 用法

```markdown
{{</* admonition type=tip title="This is a tip" open=true */>}}
一个"技巧"横幅
{{</* /admonition */>}}

或者

{{</* admonition tip "This is a tip" true */>}}
一个"技巧"横幅
{{</* /admonition */>}}
```

#### 示例

{{< admonition type=note title="注意" open=true >}}
type=note
{{< /admonition >}}

{{< admonition type=abstract title="摘要" open=true >}}
type=abstract
{{< /admonition >}}

{{< admonition type=info title="信息" open=true >}}
type=info
{{< /admonition >}}

{{< admonition type=tip title="技巧" open=true >}}
type=tip
{{< /admonition >}}

{{< admonition type=success title="成功" open=true >}}
type=success
{{< /admonition >}}

{{< admonition type=question title="问题" open=true >}}
type=question
{{< /admonition >}}

{{< admonition type=warning title="警告" open=true >}}
type=warning
{{< /admonition >}}

{{< admonition type=failure title="失败" open=true >}}
type=failure
{{< /admonition >}}

{{< admonition type=danger title="危险" open=true >}}
type=danger
{{< /admonition >}}

{{< admonition type=bug title="Bug" open=true >}}
type=Bug
{{< /admonition >}}

{{< admonition type=example title="示例" open=true >}}
type=example
{{< /admonition >}}

{{< admonition type=quote title="引用" open=true >}}
type=quote
{{< /admonition >}}
