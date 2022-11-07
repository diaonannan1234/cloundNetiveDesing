# 自定义DSL优化权限规则设计
>
> 设计权限规则DSL语言。
>
> 方便业务人员、运维人员设置权限，包括数据权限。
>
> 例如： if r.user.dept == '财务部' then pass  elseif  r.user.isLeder == true then unpass
> r  代表 当前请求，可以在请求中获取当前登录用户、用户部门、当前实体如“合同实体”
>
>
>
>
>
>
>
>
>