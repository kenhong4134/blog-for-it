---
title: "???"
date: 2022-09-16T00:20:51+08:00
draft: true
---


Entity Framework 
	Database First, Model First, Code First
	


設計 View Model
Data Annotation 顯示與驗證資料

Model Binder




Razor 語法
	- 使用 Html Helper
	- Partial Views
	
	

Controller & Action
	- MVC 請求處理方法
	- 撰寫 Controller Action
	- Action Filter
	
			

	Action method Return 回傳值
	- ActionResult
	ViewResult
	HttpNotFoundResult
	RedirectToRouteResult
	ContentResult
	EmptyResult
	FileContentResult


	傳遞資料到 View
		- View() helper
	沒有 Model，傳遞資料到 View
		- ViewBag
			- Dynamic Expression
		- ViewData
			- Dictionary 物件
			- 向下相容
			
		
	- Async & Await
		同步與非同步控制器
		
	- Action Filter
		- Cross-cutting concerns
			- Authorization
			- Logging
			- Caching
		- Built-in Action Filter
			- Authorize
			- Action
			- HandleError
			



- 驗證與授權
	- Authentication
		- 使用 Credenital 證明你是誰
	- Authorization
		- 用戶端是否有權限存取要求的資源
		
- 支援的驗證類型
	- Individual User Accounts
		- ASP.NET Identity
	- Oranaizational Accounts
		- Windows Servcie AD or Azure Directory
	- Windows Authentications
		- Intranet
		
- 狀態管理		
	- TempData
	- Session
