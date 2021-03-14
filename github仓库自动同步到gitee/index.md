# GitHub仓库自动同步到Gitee


## 设置 `dst_key` 

1. 在 `GitHub` 上打开自己的一个仓库（可以是要同步的源仓库，也可以是别的仓库，总之这个仓库将用来执行GitHub Actions），在它的 `setting` → `Secrets` 中，新建一个 `GITEE_PRIVATE_KEY` 。
2. 在电脑上生成SSH密钥对。
3. 复制私钥 `id_rsa` 的值，粘贴为第1步 `GITEE_PRIVATE_KEY` 的 `Value` 。
3. 复制公钥 `id_rsa.pub` 的值。打开 `Gitee` ，在个人 `设置` → `安全设置` → `SSH公钥` 中新添加公钥，标题无所谓，将公钥 `id_rsa.pub` 的值粘贴上。

## 设置 `dst_token` 

1. 在刚才的 `GitHub` 仓库的 `setting` → `Secrets` 中，再新建一个 `GITEE_TOKEN` 。
2. 打开 `Gitee` ，在个人 `设置` → `安全设置` → `私人令牌` 中新建一个私人令牌，名字无所谓，全选权限，确定，然后复制它的值，粘贴为第1步中 `GITEE_TOKEN` 的 `Value` 。

## 配置 `Github Actions` 

在刚才的GitHub仓库中，新建 `.github/workflows/SyncToGitee.yml` 文件，写入以下代码，然后提交：

```yaml
name: Sync Github Repos To Gitee

on:
  push:
    branches:
      - master

jobs:
  build:
    runs-on: ubuntu-20.04
    steps:

    - name: Sync Github Repos To Gitee #名字随便起
      uses: Yikun/hub-mirror-action@v1.0 #使用Yikun开发的hub-mirror-action
      with:
        src: github/Repo_name #源仓库地址
        dst: gitee/Repo_name #目标仓库地址
        dst_key: ${{ secrets.GITEE_PRIVATE_KEY }} #SSH密钥对中的私钥，即 id_rsa
        dst_token:  ${{ secrets.GITEE_TOKEN }} #Gitee账户的私人令牌
        account_type: user #账户类型
        clone_style: "ssh" #使用ssh方式进行clone，也可以使用https
        debug: true #启用后会显示所有执行命令
        force_update: true #启用后，强制同步，即强制覆盖目的端仓库
        static_list: "picture" #静态同步列表，在此填写需要同步的仓库名称
        timeout: '600s' #git超时设置，超时后会自动重试git操作
```

{{< admonition type=note title="注意" open=true >}}
1. 这个配置文件仅适用于 `GitHub` 中的源仓库在每次提交更改后，自动将源仓库同步到 `Gitee` 的目标仓库。

2. 也可以使用 `cron` 命令使其定时运行：

```yaml
on:
  schedule:
    - cron: '0 1 * * *'  #每天北京时间 9点运行
    branches:
      - master
```

3. 源仓库与目标仓库名称应该设置为相同，至于名称不同的解决方法，作者Yikun暂时还没有实现，参考这个 [issue](https://github.com/Yikun/hub-mirror-action/issues/64) 
4. 至于将 `Gitee` 中的源仓库同步到 `GitHub` 的目标仓库，大概只需要将 `.github/workflows/SyncToGitee.yml` 中源仓库地址 `src` 和目标仓库地址 `dst` 设置一下即可？我试试再说🤔

{{< /admonition >}}

</br>

## 参考链接
- [Yikun/hub-mirror-action](https://github.com/Yikun/hub-mirror-action)
- [yi-Xu-0100/hub-mirror](https://github.com/yi-Xu-0100/hub-mirror)

