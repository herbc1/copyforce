# CopyForce Command Line -- All Database Versions #

This page documents the command line syntax for all versions of CopyForce.

The switches common to all database versions are listed first, followed by database
specific switches.


# Common Switches #

The following command line switches are supported by all concrete implementations of CopyForce:

<table border='2'>
<blockquote><tr align='left' valign='top'>
<th>Command Line Switch</th><th>Description</th>
</tr>
<tr align='left' valign='top'><td>-salesforce string</td>
<td>Salesforce database from which data will be copied. This value can be a:<br>
<ul>
<li>Name of a Salesforce profile defined in $HOME/sqlforce.ini</li>
<li>Comma separated string containing [Production||Sandbox],Username,Password,SecurityToken</li>
</ul>
Examples:<br>
<ul>
<li>-salesforce Sandbox</li>
<li>-salesforce Production,gsmithfarmer@gmail.com,holyMoly,99898aasf89asf</li>
</ul>
</td>
</tr></blockquote>

<blockquote><tr align='left' valign='top'><td>-schema</td>
<td>Create the Salesforce schema in the destination databases. Note that if a schema object already exists in the destination then CopyForce<br>
will skip it automatically.<br>
</td>
</tr></blockquote>

<blockquote><tr align='left' valign='top'><td>-include string</td>
<td>Comma separated list of tables (or regular expressions) to copy from Salesforce. By default, all tables all included. Examples:<br>
<ul>
<li>-include "Account,Contact"</li>
<li>-include "Prod.<code>*</code>,Price.<code>*</code>"</li>
</ul>
</td>
</tr></blockquote>


<blockquote><tr align='left' valign='top'><td>-exclude string</td>
<td>Comma separated list of tables (or regular expressions) to EXCLUDE from the copy from Salesforce. By default, no tables are excluded. Examples:<br>
<ul>
<li>-exclude "Attachment"</li>
<li>-exclude "Attachment,Software.<code>*</code>"</li>
</ul>
</td>
</tr></blockquote>

<blockquote><tr align='left' valign='top'><td>-config file</td>
<td>XML file that specifies what to copy/exclude from Salesforce. This file should be used when the simple <i>-include</i> and <i>-exclude</i> are insufficient<br>
to represent the parameters for the copy. See later in this documentation for the format of a configuration file.<br>
</td>
</tr></blockquote>

<blockquote><tr align='left' valign='top'><td>-gui</td>
<td>Display a UI that indicates the progress of the copy. By default, progress is written to stdout.<br>
</td>
</tr></blockquote>

<blockquote><tr align='left' valign='top'><td>-silent</td>
<td>Do not display progress messages.<br>
</td>
</tr></blockquote>

<blockquote><tr align='left' valign='top'><td>-timeout ms</td>
<td>Set the timeout value for calls to Salesforce. The default is 1,000,000 and rarely needs to changed.<br>
</td>
</tr></blockquote>

<blockquote><tr align='left' valign='top'><td>-buffer mBytes</td>
<td>Set the number of megabytes CopyForce will buffer from Salesforce before writing them to the destination database.<br>
</td>
</tr>
</blockquote><blockquote></table></blockquote>

# SQL Server Additional Switches #
The CopyForceSqlServer.jar version adds the following command line switches.
<table border='2'>
<blockquote><tr align='left' valign='top'>
<th>Command Line Switch</th><th>Description</th>
</tr>
<tr align='left' valign='top'><td>-sqlserver string</td>
<td>Connection string for accessing SQL Server.<br>
Examples:<br>
<ul>
<li>-sqlserver  "//localhost;databaseName=sqlforcetest;username=me;password=caleb&noah;"</li>
</ul>
Internally, the connection string will be passed to the SQL Server <i>DriverManager.getConnection()</i> method (prefixed by <i>jdbc:sqlserver:</i>).<br>
</td>
</tr>
</table></blockquote>

# H2 Additional Switches #
The CopyForceH2.jar version adds the following command line switches.
<table border='2'>
<blockquote><tr align='left' valign='top'>
<th>Command Line Switch</th><th>Description</th>
</tr>
<tr align='left' valign='top'><td>-h2 string</td>
<td>Connection string for accessing an existing (or creating a new) H2 database.<br>
Examples:<br>
<ul>
<li>-h2 C:/tmp/practiceH2</li>
</ul>
</td>
</tr></blockquote>

<blockquote><tr align='left' valign='top'><td>-h2user string</td>
<td>User name for the H2 database.<br>
</td>
</tr></blockquote>

<blockquote><tr align='left' valign='top'><td>-h2password string</td>
<td>Password for the H2 database.<br>
</td>
</tr>
</table>
<h1>Configuration Files</h1>
The list of objects copied from Salesforce can optionally be controlled by a configuration file (see the -config command line switch). A configuration file<br>
</blockquote>> must be in the following form:
```
  <CopyForce>
  	<include table="nameMatchingPattern" where="optional Where Clause" >
  	<exclude table="nameMatchingPattern" >
  </CopyForce>
```
> where:
  * The `<include>` element indicates tables that will be include in the copy. The name attribute is a regular expression that will be used to find matching
> table names (a plain table name (like Account) is, of course, a regular expression. The optional <i>where</i> attribute is should be a string that can be appended to a
> <i>WHERE</i> clause for SOQL against the table.
  * The `<exclude>` element indicates tables that should be excluded from the copy.

Example: Copy all tables except Attachments:
```
  <CopyForce>
  	<include table=".*"  />
  	<exclude table="Attachment" />
  </CopyForce>
```