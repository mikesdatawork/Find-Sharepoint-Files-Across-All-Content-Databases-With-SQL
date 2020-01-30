![MIKES DATA WORK GIT REPO](https://raw.githubusercontent.com/mikesdatawork/images/master/git_mikes_data_work_banner_01.png "Mikes Data Work")        

# Find Sharepoint Files Across All Content Databases With SQL
**Post Date: June 14, 2018**        



## Contents    
- [About Process](##About-Process)  
- [SQL Logic](#SQL-Logic)  
- [Build Info](#Build-Info)  
- [Author](#Author)  
- [License](#License)       

## About-Process


![Find Sharepoint Files With SQL]( https://mikesdatawork.files.wordpress.com/2018/06/image001.png "Get Sharepoint Files With SQL")
 
<p>In the example below you'll find some basic SQL logic that allows you to search for documents across all Sharepiont Content Databases.</p> 



## SQL-Logic
```SQL
use master;
set nocount on
 
declare @gather     varchar(max)
set @gather     = ''
select  @gather     = @gather + 
'
use [' + [name] + '];' + char(10) + 
'set nocount on' + char(10) + 
'select 
    ''database''        = db_name()
,   ''time_created''    = alldocs.timecreated
,   ''list_name''       = alllists.tp_title
,   ''file_name''       = alldocs.leafname
,   ''url''         = alldocs.dirname
,   ''kb''          = (convert(bigint,alldocstreams.size))/1024
,   ''mb''          = (convert(bigint,alldocstreams.size))/1024/1024
from 
    alldocs join alldocstreams  on alldocs.id=alldocstreams.id 
    join alllists           on alllists.tp_id = alldocs.listid
where
    right([alldocs].[leafname], 2) in (''oc'', ''cx'', ''df'', ''sg'', ''xt'')
    and alldocs.leafname like ''%design%''
order by
    alldocs.leafname asc
' + char(10) + char(10)
from    sys.databases where [name] like '%content%'
 
if object_id('tempdb..##gathersummary') is not null
    drop table  ##gathersummary
create table        ##gathersummary
(
    [database]  varchar(255)
,   [time_created]  datetime
,   [list_name] varchar(555)
,   [file_name] varchar(555)
,   [url]       varchar(max)
,   [kb]        int
,   [mb]        int
)
 
insert into ##gathersummary
exec    (@gather) 
 
select * from ##gathersummary where [file_name] like '%MyWildCardSearch%'
```


[![WorksEveryTime](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://shitday.de/)

## Build-Info

| Build Quality | Build History |
|--|--|
|<table><tr><td>[![Build-Status](https://ci.appveyor.com/api/projects/status/pjxh5g91jpbh7t84?svg?style=flat-square)](#)</td></tr><tr><td>[![Coverage](https://coveralls.io/repos/github/tygerbytes/ResourceFitness/badge.svg?style=flat-square)](#)</td></tr><tr><td>[![Nuget](https://img.shields.io/nuget/v/TW.Resfit.Core.svg?style=flat-square)](#)</td></tr></table>|<table><tr><td>[![Build history](https://buildstats.info/appveyor/chart/tygerbytes/resourcefitness)](#)</td></tr></table>|

## Author

[![Gist](https://img.shields.io/badge/Gist-MikesDataWork-<COLOR>.svg)](https://gist.github.com/mikesdatawork)
[![Twitter](https://img.shields.io/badge/Twitter-MikesDataWork-<COLOR>.svg)](https://twitter.com/mikesdatawork)
[![Wordpress](https://img.shields.io/badge/Wordpress-MikesDataWork-<COLOR>.svg)](https://mikesdatawork.wordpress.com/)

 
## License
[![LicenseCCSA](https://img.shields.io/badge/License-CreativeCommonsSA-<COLOR>.svg)](https://creativecommons.org/share-your-work/licensing-types-examples/)

![Mikes Data Work](https://raw.githubusercontent.com/mikesdatawork/images/master/git_mikes_data_work_banner_02.png "Mikes Data Work")

