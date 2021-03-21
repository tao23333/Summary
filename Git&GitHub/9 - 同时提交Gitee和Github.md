> 现在想把文件提交到Gitee，其命令操作和GitHub类似

- 使用ssh进行推送，找到ssh密码（之前设置的是系统级别签名，一台机器就一个）
  - `cd ~/.ssh`
  - `cat id_rsa.pub`
  - 复制密钥粘贴到Gitee -> 设置 -> 新建SSH公钥 -> 添加公钥
- 在Gitee新建一个空仓库(什么都没有，包括README和开源协议等)，名称与要上传的目录相同，复制该仓库的ssh地址
- 创建远程地址别名（若当前项目也向GitHub提交，注意别命不要重复）
  - `git remote add origin_gitee master`
- 本地库内容提交至Gitee
  - `git push origin_gitee master`