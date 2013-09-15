---
layout: post
title: "Formatting those sql_exec profiler outputs.."
category: Development
tags: [sql, projects]
summary: A little tool I wrote to format sql outputs with sql_exec gunk
---


I was debugging some long running queries the other day, using SQL Server profiler (BTW - here is a very decent free equivalent of this for those of you using SQL Express - which doesn't come with the profiler: [sqlprofiler](https://sites.google.com/site/sqlprofiler) ) .

Anyhoo, one thing that has always been a pain for me is that the output you get from a sql_exec command - which is how most ORMs execute SQL against the database. Take this example command:

	exec sp_executesql N'SELECT [t1].[CableSweepChangeId], [t1].[FrontEndId], [t1].[FrontEndPortNum], [t1].[IncomingTime], [t1].[Description], [t1].[ChangePoint], [t1].[OldLength], [t1].[NewLength], [t1].[EquipmentId], [t1].[DataLinkStateChange], [t1].[IsLink], [t1].[IsConnection], [t1].[IsLaptopChange], [t1].[EquipmentPortNum], [t1].[EquipmentName], [t1].[CableId], [t1].[CableLabel], [t1].[InstallType], [t1].[ConnectionSide]
	FROM (
	    SELECT ROW_NUMBER() OVER (ORDER BY [t0].[IncomingTime] DESC) AS [ROW_NUMBER], [t0].[CableSweepChangeId], [t0].[FrontEndId], [t0].[FrontEndPortNum], [t0].[IncomingTime], [t0].[Description], [t0].[ChangePoint], [t0].[OldLength], [t0].[NewLength], [t0].[EquipmentId], [t0].[DataLinkStateChange], [t0].[IsLink], [t0].[IsConnection], [t0].[IsLaptopChange], [t0].[EquipmentPortNum], [t0].[EquipmentName], [t0].[CableId], [t0].[CableLabel], [t0].[InstallType], [t0].[ConnectionSide]
	    FROM [CableChangeDetail] AS [t0]
	    ) AS [t1]
	WHERE [t1].[ROW_NUMBER] BETWEEN @p0 + 1 AND @p0 + @p1
	ORDER BY [t1].[ROW_NUMBER]',N'@p0 int,@p1 int',@p0=0,@p1=15

Now, if I want to tweak this or play around with it, it's not much fun having it in this format.. I would prefer it in a format with all those params as DECLARE statements at the top. Which is why I wrote, and present to you now..... SQLEXEC FORMATTER!!!

## [http://execsqlformat.com](http://execsqlformat.com)

Here it is in action:

![book](http://dl.dropbox.com/u/292924/blog/execsqlformat.PNG)

At the moment the copy to clipboard button is a bit bad at formatting the text, but I'll sort that one day, and you can always just copy the text manually.

Enjoy!
