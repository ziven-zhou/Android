# AndroidR原生Launcher UI加载流程

## 桌面布局结构

- 桌面的主要组成：
  - 主页面：Workspace、WorkspacePageIndicator、Hotseat

  - 桌面元素长按拖动时顶部的删除区域：DropTargetBar

  - 全部应用页面：LauncherAllAppsContainerView

  - 最近使用界面：LauncherRecentView、OverViewActionsView，如图：

- 桌面布局的层级：
	- 跟布局(LauncherRootView)
	
	- 拖动层(DrayLayer)：协调上层布局的拖动

	- 工作空间(Workspace)：壁纸及CellLayout页面管理
	
	- 工作单元(CellLayout)：Item/Widget/Folder所属区域页面管理
	
	- 原子布局展示区域(ShortcutAndWidgetContainer)：管理Item/Widget/Folder，如图：

## Launcher数据加载过程
- Launcher数据加载/更新的触发条件：
	- Launcher Activity的创建(Launcher.onCreate)
	
	- 布局结构/idp发生变化(onConfigurationChanged/onIdpChanged)
	
	- 强制性更新(LauncherModel.forceReload)：桌面数据库更新后重新加载/绑定工作空间
	
	- Launcher Activity的销毁(Launcher.onDestroy)：移除当前Launcher Activity的回调，重新加载其它回调，如图:

	执行数据更新/绑定前会先清除之前存在的延迟绑定任务，并停止正在加载的任务，然后判断是否数据有更新、数据更新是否结束，如果是,则只执行数据绑定LoaderResults操作，即完成数据-UI的绑定；如果否，则执行数据加载LoaderTask，同时完成数据绑定LoaderResults操作。
  
- 对应关系：
	 - LoaderTask中的loadWorkspace对应LoaderResults中的bindWorkspace，完成Workspace工作空间的数据加载/UI绑定
	 - LoaderTask中的loadAllApps对应LoaderResults中的bindAllApps，完成全部应用页面的数据加载/UI绑定
	 - LoaderTask中的loadDeepShortcuts对应LoaderResults中的bindDeepShortcuts
	 - WidgetsModel中的update对应LoaderResults中的bindWidgets
  
  
