# MIS-6
##查询用户test1可以查看的页面（Sys_menu）

伪代码
根据名称查找人员编号UserID 之后根据人员编号UserID查找改人员所对应的角色集合RoleIDs
权限表 LEFT JOIN 页面表并查找
   角色类型为CF_Role   AND   角色编号在角色集合RoleIDs中
  OR
   角色类型为CF_User   AND   人员编号为UserID
  AND权限属性Permit   AND   权限为Sys_Menu的数据
  
  
关键代码 

LEFT JOIN sys_menu AS sm ON cp.PrivilegeAccessKey = sm.MenuID AND cp.PrivilegeAccess = 'Sys_Menu'
 WHERE ((cp.PrivilegeMaster = 'CF_Role'
     AND cp.PrivilegeMasterKey
     IN (SELECT RoleID FROM cf_userrole AS cur
       LEFT JOIN cf_user AS cu ON cur.UserID = cu.UserID WHERE cu.LoginName='test1' ))
    OR
     (cp.PrivilegeMaster = 'CF_User'
     AND cp.PrivilegeMasterKey = (SELECT UserID FROM cf_user WHERE LoginName = 'test1')))
    AND
     cp.PrivilegeOperation = 'Permit' AND cp.PrivilegeAccess = 'Sys_Menu';
![image](https://cloud.githubusercontent.com/assets/16076963/19844449/e949ce8c-9f67-11e6-9e49-25dd489c8ec1.png)


#对订单(order)页面中的操作权限(sys_button)

伪代码
根据名称查找人员编号UserID 根据人员编号UserID查找改人员所对应的角色集合RoleIDs
权限表 LEFT JOIN 按钮表 LEFT JOIN 页面表并查找
   角色类型为CF_Role   AND   角色编号在角色集合RoleIDs中
  OR
   角色类型为CF_User   AND   人员编号为UserID
  AND 权限属性为Permit   AND   权限为Sys_Button AND 菜单名字为订单的数据
  
  关键代码
  LEFT JOIN sys_button AS sb ON cp.PrivilegeAccessKey = sb.BtnID AND cp.PrivilegeAccess = 'Sys_Button'
  LEFT JOIN sys_menu AS sm ON sb.MenuNo = sm.MenuNo
 WHERE ((cp.PrivilegeMaster = 'CF_Role'
     AND cp.PrivilegeMasterKey
     IN (SELECT RoleID FROM cf_userrole AS cur
       LEFT JOIN cf_user AS cu ON cur.UserID = cu.UserID WHERE cu.LoginName='test1' ))
    OR
     (cp.PrivilegeMaster = 'CF_User'
     AND cp.PrivilegeMasterKey = (SELECT UserID FROM cf_user WHERE LoginName = 'test1')))
    AND
     cp.PrivilegeOperation = 'Permit' AND cp.PrivilegeAccess = 'Sys_Button' AND sm.MenuName = '订单';
     
   ![image](https://cloud.githubusercontent.com/assets/16076963/19844709/74e514d6-9f6a-11e6-9c24-efed577c5153.png)
