# The Development and Distribution for this project has been moved to the [SQLForce](http://code.google.com/p/sqlforce/) project #
CopyForce contains tools for copying full or partial Salesforce instances to a relational database. Several target databases are supported out of the box:
  * Microsoft SQL Server
  * H2

Example: Copy all tables from Salesforce to an empty SQL Server database.
```
java -jar CopyForceSqlServer.jar -schema -gui 
  -salesforce Production,myname@gmail.com,password,0908989token
  -sqlserver "//localhost;databaseName=sqlforcetest;username=me;password=caleb&noah;" 
```

Example: Update existing Account and Contact tables in SQL Server.
```
java -jar CopyForceSqlServer.jar -include "Account,Contact" -gui 
  -salesforce Production,myname@gmail.com,myPassword,SecurityToken  
  -sqlserver "//localhost;databaseName=sqlforcetest;username=me;password=caleb&noah;"
```

Example: Create a new H2 database that contains all Saleforce tables.
```
java -jar CopyForceH2.jar -schema -gui 
  -salesforce Production,myname@gmail.com,myPassword,SecurityToken 
  -h2 C:/tmp/SalesforceCopy -h2user sa -h2password aslan 
```

The source for this project can be found at http://code.google.com/p/sqlforce/.

If it works for you and or you have suggestions please contact me at [mailto:gsmithfarmer@gmail.com](mailto:gsmithfarmer@gmail.com). If you need support for another database engine, I would be glad to help.