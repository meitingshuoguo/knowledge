在提交代码的时候使用 husky

个人认为要在这里做 eslint 和 prettier 两个插件的自动修复，如有错误，则不能提交。
因为这里是最后把关的地方。在编写阶段，甚至可以不装扩展，不使用保存时就自动格式化的功能。只要保证在提交之前能通过验证就没有问题。给予编写过程中个人极大的自由。

vue-cli 在创建项目的时候就会问是要在保存的时候 lint 还是提交的时候 lint 和修复。
Lint on save
Lint and fix on commit
eslint --fix
prettier --write
