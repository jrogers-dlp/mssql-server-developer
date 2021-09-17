# mssql-server-windows-developer
This is the Dockerfile for the latest version of SQL Server 2019 Developer Edition for Windows Containers.

You can build the container with the following command.

````
docker build -t mssql-server-developer:latest .
````

You can run the container with the following command.

````
docker run --name sqldev -d -p 1433:1433 -e ACCEPT_EULA=Y -e sa_password=MyPass!23 mssql-server-developer:latest
````

- **-p HostPort:containerPort** is for port-mapping a container network port to a host port.
- **-v HostPath:containerPath** is for mounting a folder from the host inside the container.

  This can be used for saving database outside of the container.

- **-it** can be used to show the verbose output of the SQL startup script.

  Use this to debug the container in case of issues.

<a name=run-this-sample></a>

## Run this sample

The image provides two environment variables to optionally set: </br>
- **accept_eula**: Confirms acceptance of the end user licensing agreement found [here](http://go.microsoft.com/fwlink/?LinkId=746388)
- **sa_password**: Sets the sa password and enables the sa login
- **attach_dbs**: The configuration for attaching custom DBs (.mdf, .ldf files).

  This should be a JSON string, in the following format (note the use of SINGLE quotes!)
  ```
  [
	{
		'dbName': 'MaxDb',
		'dbFiles': ['C:\\temp\\maxtest.mdf',
		'C:\\temp\\maxtest_log.ldf']
	},
	{
		'dbName': 'PerryDb',
		'dbFiles': ['C:\\temp\\perrytest.mdf',
		'C:\\temp\\perrytest_log.ldf']
	}
  ]
  ```

  This is an array of databases, which can have zero to N databases.

  Each consisting of:
  - **dbName**: The name of the database
  - **dbFiles**: An array of one or many absolute paths to the .MDF and .LDF files.

	**Note:**
	The path has double backslashes for escaping!
	The path refers to files **within the container**. So make sure to include them in the image or mount them via **-v**!


This example shows all parameters in action:
```
docker run -d -p 1433:1433 -v C:/temp/:C:/temp/ -e sa_password=<YOUR SA PASSWORD> -e ACCEPT_EULA=Y -e attach_dbs="[{'dbName':'SampleDB','dbFiles':['C:\\temp\\sampledb.mdf','C:\\temp\\sampledb_log.
ldf']}]" microsoft/mssql-server-windows-developer
```

<a name=sample-details></a>

## Rebuild the image

```
docker build --memory 4g .
```


<a name=related-links></a>

## Related Links
<!-- Links to more articles. Remember to delete "en-us" from the link path. -->

For more information, see these articles:
- [Windows Containers](https://msdn.microsoft.com/en-us/virtualization/windowscontainers/about/about_overview)
- [Windows-based containers: Modern app development with enterprise-grade control](https://www.youtube.com/watch?v=Ryx3o0rD5lY&feature=youtu.be)
- [Windows Containers: What, Why and How](https://channel9.msdn.com/Events/Build/2015/2-704)
- [SQL Server in Windows Containers](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/03/21/sql-server-in-windows-containers/#comments)
