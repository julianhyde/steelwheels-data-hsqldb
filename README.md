<!--
{% comment %}
Licensed to Julian Hyde under one or more contributor license
agreements.  See the NOTICE file distributed with this work
for additional information regarding copyright ownership.
Julian Hyde licenses this file to you under the Apache
License, Version 2.0 (the "License"); you may not use this
file except in compliance with the License.  You may obtain a
copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing,
software distributed under the License is distributed on an
"AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
either express or implied.  See the License for the specific
language governing permissions and limitations under the
License.
{% endcomment %}
-->
[![Build Status](https://github.com/julianhyde/steelwheels-data-hsqldb/actions/workflows/main.yml/badge.svg?branch=main)](https://github.com/julianhyde/steelwheels-data-hsqldb/actions?query=branch%3Amain)
[![Maven Central](https://maven-badges.herokuapp.com/maven-central/net.hydromatic/steelwheels-data-hsqldb/badge.svg)](https://maven-badges.herokuapp.com/maven-central/net.hydromatic/steelwheels-data-hsqldb)

# steelwheels-data-hsqldb
Steelwheels data set in hsqldb format

This project contains the Steelwheels data set as an embedded
HSQLDB database.

It originated as part of the test suite of the
<a href="http://mondrian.pentaho.org">Pentaho Mondrian OLAP engine</a>.

# Schema

Steelwheels contains 13 tables:

| Table | Row count |
| ----- | --------: |
| customer_w_ter | 126 |
| customers | 126 |
| department_managers | 4 |
| employees | 23 |
| offices | 7 |
| orderdetails | 3,001 |
| orderfact | 2,996 |
| orders | 330 |
| payments | 272 |
| products | 110 |
| quadrant_actuals | 148 |
| time | 265 |
| trial_balance | 22 |

Its size is about 1MB uncompressed, 124KB compressed.

Here is a schema diagram:

![Steelwheels schema diagram](steelwheels-schema.png)

# Using the data set

The data set is packaged as a jar file that is published to
[Maven Central](https://search.maven.org/#search%7Cga%7C1%7Ca%3Asteelwheels-data-hsqldb)
as a Maven artifact. To use the data in your Java application,
add the artifact to your project's dependencies:

```xml
<dependency>
  <groupId>net.hydromatic</groupId>
  <artifactId>steelwheels-data-hsqldb</artifactId>
  <version>0.2</version>
</dependency>
```

Now you can connect using Java code:

```java
import java.sql.Connection;

final String url = "jdbc:hsqldb:res:steelwheels";
final String user = "STEELWHEELS";
final String password = "STEELWHEELS";
final String sql = "select lastname"\n"
    + "from \"steelwheels\".\"employees\"";
try (Connection c = DriverManager.getConnection(url, user, password);
    Statement s = c.createStatement();
    ResultSet r = s.executeQuery(sql)) {
  while (r.next()) {
    System.out.println(r.getString(1));
  }
}
```

You can also connect using a JDBC interface such as [sqlline](https://github.com/julianhyde/sqlline).
Make sure that `steelwheels-data-hsqldb.jar` is on the class path, and start `sqlline`:

```sql
$ ./sqlline
sqlline version 1.12.0
sqlline> !connect jdbc:hsqldb:res:steelwheels sa ""
0: jdbc:hsqldb:res:steelwheels> select count(*) from "steelwheels"."employees";
+----------------------+
|          C1          |
+----------------------+
| 23                   |
+----------------------+
1 row selected (0.004 seconds)
0: jdbc:hsqldb:res:steelwheels> !quit
```

If you use username and password "STEELWHEELS" and "STEELWHEELS", the
default schema is "steelwheels", so you can omit the table prefix, if
you wish:

```sql
$ ./sqlline
sqlline version 1.12.0
sqlline> !connect jdbc:hsqldb:res:steelwheels STEELWHEELS STEELWHEELS
0: jdbc:hsqldb:res:steelwheels> select count(*) from "employees";
+----------------------+
|          C1          |
+----------------------+
| 23                   |
+----------------------+
1 row selected (0.004 seconds)
0: jdbc:hsqldb:res:steelwheels> !quit
```

## Get steelwheels-data-hsqldb

### From Maven

Get steelwheels-data-hsqldb from
<a href="https://search.maven.org/#search%7Cga%7C1%7Cg%3Anet.hydromatic%20a%3Asteelwheels-data-hsqldb">Maven Central</a>:

```xml
<dependency>
  <groupId>net.hydromatic</groupId>
  <artifactId>steelwheels-data-hsqldb</artifactId>
  <version>0.2</version>
</dependency>
```

### Download and build

Java version 8 or higher.

```bash
$ git clone git://github.com/julianhyde/steelwheels-data-hsqldb.git
$ cd steelwheels-data-hsqldb
$ ./mvnw install
```

+On Windows, the last line is

```bash
> mvnw install
```

### Make a release

See [hydromatic-parent](https://github.com/julianhyde/hydromatic-parent).

## See also

Similar data sets:
* [chinook-data-hsqldb](https://github.com/julianhyde/chinook-data-hsqldb)
* [foodmart-data-hsqldb](https://github.com/julianhyde/foodmart-data-hsqldb)
* [scott-data-hsqldb](https://github.com/julianhyde/scott-data-hsqldb)
* [steelwheels-data-hsqldb](https://github.com/julianhyde/steelwheels-data-hsqldb)

## More information

* License: Apache License, Version 2.0
* Author: Julian Hyde
* Blog: http://blog.hydromatic.net
* Project page: http://www.hydromatic.net/steelwheels-data-hsqldb
* Source code: https://github.com/julianhyde/steelwheels-data-hsqldb
* Distribution: <a href="https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22steelwheels-data-hsqldb%22">Maven Central</a>
* Developers list:
  <a href="mailto:dev@calcite.apache.org">dev at calcite.apache.org</a>
  (<a href="https://mail-archives.apache.org/mod_mbox/calcite-dev/">archive</a>,
  <a href="mailto:dev-subscribe@calcite.apache.org">subscribe</a>)
* Issues: https://github.com/julianhyde/steelwheels-data-hsqldb/issues
* <a href="HISTORY.md">Release notes and history</a>
