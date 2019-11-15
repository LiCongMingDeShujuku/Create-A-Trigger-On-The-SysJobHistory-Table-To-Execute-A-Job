![CLEVER DATA GIT REPO](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/0-clever-data-github.png "李聪明的数据库")

# 在SysJobHistory表格上创建触发器以执行作业
#### Create A Trigger On The SysJobHistory Table To Execute A Job
**发布-日期: 2015年07月15日 (评论)**

![#](images/li-congming-software-engineer-200x200.png?raw=true "#")

## Contents
<img align="right" width="100" height="100" src="https://github.com/LiCongMingDeShujuku/git-resources/blob/master/images/li-congming-software-engineer-200x200.png">
- [中文](#中文)
- [English](#English)
- [SQL Logic](#Logic)
- [Build Info](#Build-Info)
- [Author](#Author)
- [License](#License) 


## 中文
这是一些将在SysJobHistory表上创建一个触发器（Trigger）的sql逻辑（logic）。 触发器用于在表格中插入 *Failure* 信息时执行作业。

## English
Here’s some sql logic that will create a Trigger on the SysJobHistory table. The Trigger is designed to execute a Job whenever a *Failure* message has been inserted into the table.

---
## Logic
```SQL
use msdb;
set nocount on
set ansi_nulls on
set quoted_identifier on
go

create trigger [dbo].[check_for_job_failure] on msdb..sysjobhistory
after insert
	as
		begin
			set nocount on
			declare @is_fail int
			set @is_fail = (select case when [message] like '%The step failed%' then 1
			else 0 
		end 
from 
msdb..sysjobhistory where instance_id in (select max(instance_id) from
msdb..sysjobhistory))
if @is_fail = 1
	begin
		exec msdb.dbo.sp_start_job @job_name = 'SEND SQL JOB ALERTS (beta)'
	end
end

```



[![WorksEveryTime](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://shitday.de/)

## Build-Info

| Build Quality | Build History |
|--|--|
|<table><tr><td>[![Build-Status](https://ci.appveyor.com/api/projects/status/pjxh5g91jpbh7t84?svg?style=flat-square)](#)</td></tr><tr><td>[![Coverage](https://coveralls.io/repos/github/tygerbytes/ResourceFitness/badge.svg?style=flat-square)](#)</td></tr><tr><td>[![Nuget](https://img.shields.io/nuget/v/TW.Resfit.Core.svg?style=flat-square)](#)</td></tr></table>|<table><tr><td>[![Build history](https://buildstats.info/appveyor/chart/tygerbytes/resourcefitness)](#)</td></tr></table>|

## Author

- **李聪明的数据库 Lee's Clever Data**
- **Mike的数据库宝典 Mikes Database Collection**
- **李聪明的数据库** "Lee Songming"

[![Gist](https://img.shields.io/badge/Gist-李聪明的数据库-<COLOR>.svg)](https://gist.github.com/congmingshuju)
[![Twitter](https://img.shields.io/badge/Twitter-mike的数据库宝典-<COLOR>.svg)](https://twitter.com/mikesdatawork?lang=en)
[![Wordpress](https://img.shields.io/badge/Wordpress-mike的数据库宝典-<COLOR>.svg)](https://mikesdatawork.wordpress.com/)

---
## License
[![LicenseCCSA](https://img.shields.io/badge/License-CreativeCommonsSA-<COLOR>.svg)](https://creativecommons.org/share-your-work/licensing-types-examples/)

![Lee Songming](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/1-clever-data-github.png "李聪明的数据库")

