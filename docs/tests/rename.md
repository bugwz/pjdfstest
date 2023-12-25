# rename 文档

> An awesome project.


## 24.t

检查在重命名一个目录时，其内部的 `".."` 链接（指向父目录的链接）是否会被正确更新，以及父目录的链接数（`nlink`）是否会发生相应的变化。

测试用例的执行阶段含义：
1. `desc="rename of a directory updates its .. link"`：定义测试描述，说明该测试用于验证重命名目录时，其`".."`链接是否会被更新。
2. 引入必要的脚本和函数，例如`misc.sh`，并设置`shell`环境。
3. 定义变量 `src_parent`、`dst_parent`、`src`和`dst`作为临时目录和子目录的名称，使用`namegen`生成。
4. 使用`expect 0 mkdir`创建源父目录`${src_parent}`和目标父目录`${dst_parent}`，并设置权限为`0755`。
5. 在`${src_parent}`目录下创建名为`${src}`的子目录，并设置权限为`0755`。
6. 获取当前工作目录并保存到变量`cdir`中。
7. 验证初始条件：源父目录`${src_parent}`的链接数`（nlink）`应为`3`（因为包含.和..两个链接），目标父目录`${dst_parent}`的链接数应为`2`（只包含`.`链接）。同时，获取`${src_parent}`的`inode`号码，并验证`${src_parent}/${src}/..`的`inode`号码是否与其相同。
使用`expect 0 rename`将`${src_parent}/${src}`重命名为`${dst_parent}/${dst}`。
8. 验证重命名后的条件：源父目录`${src_parent}`的链接数应减少为`2`（因为失去了`${src}`目录的``..`链接），目标父目录`${dst_parent}`的链接数应增加为`3`（增加了`${dst}`目录的`..`链接）。同时，获取`${dst_parent}`的`inode`号码，并验证`${dst_parent}/${dst}/..`的`inode`号码是否与其相同。
10. 切换回原始工作目录。
11. 使用`expect 0 rmdir`删除`${dst_parent}/${dst}`、`${dst_parent}`和`${src_parent}`目录。