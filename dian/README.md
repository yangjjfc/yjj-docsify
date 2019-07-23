# 权限篇

> 后台系统，需求是菜单权限，按钮权限需要动态化，且可以在平台端实现可配置化，包括菜单名称，菜单的层级，菜单图标，挂载的模块，都可以随意配置，动态显示。

>我们的菜单分为四级，一级菜单表示应用，如（供应链，SPD，）是一些大的应用，scs相当于多个系统翻集合，一个容器，类似于微信，可挂载N个小程序，其他以下就是显示的左边菜单。二级算是大模块（采购，发货，企业入驻，证照管理等），三级是模块下面的一个分类，四级是真正对应的某个模块；

* 如下图显示（假图，哈哈哈~）
![图片alt](./img/1.gif)



## 效果展示

## 代码设计
1.利用vue-router中的beforeEach实现权限列表加载
```js
// register global progress.
const whiteList=['/login', '/register', '/perfectRegister', '/basic/enterprise']
router.beforeEach((to, from, next) => {
    if ( whiteList.includes(to.path) || /^\/static/g.test(to.path)) { //如果是白名单中的就可以直接跳转进该页面
        next();
    } else {
        if (!store.getters.menuList) { //查看store中是否存在menuList
            // 判断当前用户是否登陆
            store.dispatch('currentUser').then(() => {
                // 没有登陆 跳到登陆页 what
                let user = store.getters.userInfo;
                if (!user || !user.enterpriseNo) {
                    next('/login');
                    return;
                }

               
                store.dispatch('getUserMenus').then(res => { // 权限不存在,获取权限
                    store.dispatch('generateRouters', res).then(list => { //生产菜单list
                        let app = [{ //将所有的页面都放到layout下，作为子组件
                            path: '',
                            component: Layout,
                            name: 'root',
                            children: list
                        }];
                        router.addRoutes(app); // 必须是数组,动态添加可访问路由表
                        next({ ...to }); // hack方法 确保addRoutes已完成
                    });
                }).catch(errs => {
                    console.info(errs);
                    // 无法获取权限则跳到登入页,无权限进入系统
                    next('/login');
                });
            });
        } else if (to.matched.length) { //有元信息直接进入 
            next();
        } else {
            next('/app/hello');
        }
    }
});
```
>这部分是全局路由拦截,跳转页面之前做的一些事,获取动态裁单

## 权限返回数据类型
<details>
<summary>权限数据</summary>

```js
{
	"apiName":"员工登陆后，获取该员工菜单按钮权限",
	"code":"SUCCESS",
	"data":{
		"menuList":[
			{
				"parentNo":".0.",
				"permissionType":"APP",
				"no":"0000096",
				"permissionUrl":"/scm/",
				"appCode":"SCM-C",
				"label":"供应链",
				"path":"/scm/",
				"children":[
					{
						"parentNo":"0000096",
						"permissionType":"MODEL",
						"no":".0.002323.",
						"funcIcon":"icon-zuoce-qiyeguanli",
						"permissionUrl":"enterprise",
						"basicPermissionNo":".0.000003.",
						"label":"企业中心",
						"path":"/scm/enterprise",
						"children":[
							{
								"parentNo":".0.002323.",
								"permissionType":"MODEL",
								"no":".0.002323.000001.",
								"funcIcon":"icon-zhunbeiliangchan",
								"permissionUrl":"/",
								"basicPermissionNo":".0.000003.000002.",
								"label":"企业信息",
								"path":"/scm//",
								"children":[
									{
										"parentNo":".0.002323.000001.",
										"permissionType":"MENU",
										"no":".0.002323.000001.000001.",
										"funcIcon":"icon-daohang-ji",
										"permissionUrl":"basic/enterprise/account",
										"basicPermissionNo":".0.000003.000002.000081.",
										"label":"我的账户",
										"path":"/scm/basic/enterprise/account",
										"checked":false,
										"permissionNo":".0.000081.",
										"permissionCodes":"scs.ecs.enterprise.findEnterpriseDetailByEnterpriseNoForSystem,ddc.uim.userAccount.updatepassword,ddc.uim.userAccount.updatemobile,ddc.uim.userAccount.updateemail,ddc.uim.userAccount.checkpassword,demo.order.selectByPK",
										"permissionCode":"menu_ddc_uim_account_user_account",
										"pageTips":"",
										"releaseNo":"0000096"
									},
									{
										"parentNo":".0.002323.000001.",
										"permissionType":"MENU",
										"no":".0.002323.000001.000002.",
										"funcIcon":"icon-daohang-ji",
										"permissionUrl":"basic/enterprise/info",
										"basicPermissionNo":".0.000003.000002.000082.",
										"label":"基本资料",
										"path":"/scm/basic/enterprise/info",
										"checked":false,
										"permissionNo":".0.000082.",
										"permissionCodes":"scs.ecs.enterprise.findEnterpriseDetailByEnterpriseNo,sdc.certRule.findList",
										"permissionCode":"menu_ddc_ecs_audited",
										"pageTips":"",
										"releaseNo":"0000096"
									},
									{
										"parentNo":".0.002323.000001.",
										"permissionType":"MENU",
										"no":".0.002323.000001.000003.",
										"funcIcon":"icon-daohang-ji",
										"permissionUrl":"basic/enterprise/address",
										"basicPermissionNo":".0.000003.000002.000083.",
										"label":"收货地址",
										"path":"/scm/basic/enterprise/address",
										"checked":false,
										"permissionNo":".0.000083.",
										"permissionCodes":"ddc.ecs.enterpriseShippingAddress.get,ddc.ecs.enterpriseShippingAddress.findList",
										"permissionCode":"menu_ddc_ecs_shipping_addr",
										"pageTips":"",
										"releaseNo":"0000096"
									},
									{
										"parentNo":".0.002323.000001.",
										"permissionType":"MENU",
										"no":".0.002323.000001.000004.",
										"funcIcon":"icon-daohang-ji",
										"permissionUrl":"basic/enterprise/bill",
										"basicPermissionNo":".0.000003.000002.000084.",
										"label":"发票助手",
										"path":"/scm/basic/enterprise/bill",
										"checked":false,
										"permissionNo":".0.000084.",
										"permissionCodes":"ddc.ecs.enterpriseInvoiceManager.findList,ddc.ecs.enterpriseInvoiceManager.get",
										"permissionCode":"menu_ddc_ecs_invoice",
										"pageTips":"",
										"releaseNo":"0000096"
									}
								],
								"checked":false,
								"permissionNo":".0.000003.000002.",
								"permissionCodes":"",
								"permissionCode":"corp_info",
								"releaseNo":"0000096"
							},
							{
								"parentNo":".0.002323.",
								"permissionType":"MODEL",
								"no":".0.002323.000002.",
								"funcIcon":"icon-zhunbeiliangchan",
								"permissionUrl":"/",
								"basicPermissionNo":".0.000003.000004.",
								"label":"岗位权限",
								"path":"/scm//",
								"children":[
									{
										"parentNo":".0.002323.000002.",
										"permissionType":"MENU",
										"no":".0.002323.000002.000001.",
										"funcIcon":"icon-daohang-ji",
										"permissionUrl":"permissions/account",
										"basicPermissionNo":".0.000003.000004.000033.",
										"label":"账号管理",
										"path":"/scm/permissions/account",
										"checked":false,
										"permissionNo":".0.000033.",
										"permissionCodes":"ddc.uim.userAccount.check,ddc.uim.userAccount.get,ddc.uim.userAccount.findListPage",
										"permissionCode":"menu_ddc_uim_account",
										"pageTips":"",
										"releaseNo":"0000096"
									},
									{
										"parentNo":".0.002323.000002.",
										"permissionType":"MENU",
										"no":".0.002323.000002.000002.",
										"funcIcon":"icon-daohang-ji",
										"permissionUrl":"permissions/staff",
										"basicPermissionNo":".0.000003.000004.000034.",
										"label":"员工管理",
										"path":"/scm/permissions/staff",
										"checked":false,
										"permissionNo":".0.000034.",
										"permissionCodes":"ddc.uim.employee.ptenterlist,ddc.uim.employee.get,ddc.uim.employee.update,ddc.uim.employee.findList,ddc.uim.employee.findListPage,ddc.uim.employeeGroup.get,ddc.uim.employeeGroup.save,ddc.uim.employeeGroup.findList,ddc.uim.employeeGroup.findListPage,ddc.uim.employee.listNotUserNo,ddc.uim.employeeGroup.listByGroup",
										"permissionCode":"menu_ddc_uim_employee",
										"pageTips":"",
										"releaseNo":"0000096"
									},
									{
										"parentNo":".0.002323.000002.",
										"permissionType":"MENU",
										"no":".0.002323.000002.000003.",
										"funcIcon":"icon-daohang-ji",
										"permissionUrl":"permissions/role",
										"basicPermissionNo":".0.000003.000004.000035.",
										"label":"角色管理",
										"path":"/scm/permissions/role",
										"checked":false,
										"permissionNo":".0.000035.",
										"permissionCodes":"ddc.uim.group.get,ddc.uim.group.update,ddc.uim.group.save,ddc.uim.group.findList,ddc.uim.group.findListPage,ddc.uim.group.remove,ddc.uim.group.findCheckedGroupList,ddc.uim.group.checkName",
										"permissionCode":"menu_ddc_uim_group",
										"pageTips":"",
										"releaseNo":"0000096"
									},
									{
										"parentNo":".0.002323.000002.",
										"permissionType":"MENU",
										"no":".0.002323.000002.000004.",
										"funcIcon":"icon-daohang-ji",
										"permissionUrl":"permissions/system",
										"basicPermissionNo":".0.000003.000004.000105.",
										"label":"集团管理",
										"path":"/scm/permissions/system",
										"checked":false,
										"permissionNo":".0.000105.",
										"permissionCodes":"ddc.uim.enterpriseSystem.findListPage",
										"permissionCode":"menu_ddc_ecs_corpsystem_manager",
										"pageTips":"",
										"releaseNo":"0000096"
									}
								],
								"checked":false,
								"permissionNo":".0.000003.000004.",
								"permissionCodes":"",
								"permissionCode":"group_auth",
								"releaseNo":"0000096"
							}
						],
						"checked":false,
						"permissionNo":".0.000003.",
						"permissionCodes":"",
						"permissionCode":"enterprise_center",
						"releaseNo":"0000096"
					}
				],
				"checked":false,
				"releaseNo":"0000096"
			},
		],
		"permissionList":[
			{
				"permissionType":"BUTTON",
				"permissionUrl":"/",
				"basicPermissionNo":".0.000127.000001.000128.000128.000001.",
				"apiCodes":"ddc.openlink.apiPre.remove",
				"parentBasicPermissionNo":".0.000127.000001.000128.",
				"parentObjNo":".0.002312.000001.000001.",
				"sortNum":1,
				"permissionNo":".0.000128.000001.",
				"permissionCode":"btn_ddc_openlink_apipre_remove",
				"pageTips":"",
				"releaseNo":"0000102",
				"permissionName":"删除"
			},
			{
				"permissionType":"BUTTON",
				"permissionUrl":"/",
				"basicPermissionNo":".0.000127.000001.000128.000128.000002.",
				"apiCodes":"ddc.openlink.TSgpServiceApi.getOpenApi,ddc.openlink.TSgpServiceApi.getApiDetail",
				"parentBasicPermissionNo":".0.000127.000001.000128.",
				"parentObjNo":".0.002312.000001.000001.",
				"sortNum":1,
				"permissionNo":".0.000128.000002.",
				"permissionCode":"btn_ddc_openlink_apipre_view",
				"pageTips":"",
				"releaseNo":"0000102",
				"permissionName":"查看"
			},
			{
				"permissionType":"BUTTON",
				"permissionUrl":"/",
				"basicPermissionNo":".0.000127.000001.000129.000129.000001.",
				"apiCodes":"ddc.openlink.connectorApply.getKf",
				"parentBasicPermissionNo":".0.000127.000001.000129.",
				"parentObjNo":".0.002312.000001.000002.",
				"sortNum":1,
				"permissionNo":".0.000129.000001.",
				"permissionCode":"btn_ddc_openlink_connectorapply_view",
				"pageTips":"",
				"releaseNo":"0000102",
				"permissionName":"查看"
			},
			{
				"permissionType":"BUTTON",
				"permissionUrl":"/",
				"basicPermissionNo":".0.000127.000001.000129.000129.000002.",
				"apiCodes":"ddc.openlink.connectorApply.getKf,ddc.openlink.connectorApply.updateKf",
				"parentBasicPermissionNo":".0.000127.000001.000129.",
				"parentObjNo":".0.002312.000001.000002.",
				"sortNum":1,
				"permissionNo":".0.000129.000002.",
				"permissionCode":"btn_ddc_openlink_connectorapply_edit",
				"pageTips":"",
				"releaseNo":"0000102",
				"permissionName":"编辑"
			}
		]
	},
	"apiCode":"ddc.uim.authorizationPermission.getEmployeeAppPermission",
	"message":"SUCCESS"
}
 ```

</details>

> 具体查看 (./mock/permission.json)


# 性能优化  



# 权限开发流程

前端设计好路由规则,json-tree(里面包含权限)。然后后端根据这个数据规则设计，添加菜单，按钮，tab页的数据。前端登入后，会返回这个权限列表，里面包含按钮，菜单和tab页，还有加载的组件地址。拿到这些数据后，前端先处理路由，将返回的菜单组合成路由，合并本地的一些静态路由。通过addRouter动态挂载到router，这个菜单就能根据返回动态显示了。由于是路由，设计到组件挂载的问题，所有访问的组件地址也是后端去配置的。在动态填在路由时，会出现全部组件一次性加载的问题，所以现将路由都指向同一个组件dashbord，然后通过这个组件在二次指向要访问的组件。组件的按钮权限，都全部挂载到vue.prototype上,然后全判断是否存在权限,才过滤按钮。



# js方面

* 节流:高频事件触发，但在n秒内只会执行一次，所以节流会稀释函数的执行频率 (throttle)
* 防抖:触发高频事件后n秒内函数只会执行一次，如果n秒内高频事件再次被触发，则重新计算时间 (debounce)
[文档概念](https://yuchengkai.cn/docs/frontend/#%E9%98%B2%E6%8A%96)
* 如果我们想获得一个变量的正确类型，可以通过 Object.prototype.toString.call(xx)。这样我们就可以获得类似 [object Type] 的字符串。 