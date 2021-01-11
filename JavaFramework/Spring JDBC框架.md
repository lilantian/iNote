Spring JDBC简介
先来看看一个JDBC的例子。我们可以看到为了执行一套SQL语句，我们需要创建链接，创建语句对象，然后执行SQL，然后操纵结果集获取数据。
```java
try(Connection connection = DriverManger.getConnection(URL, USERNAME, PASSWORD)){
	List<User> users = new ArrayList<>();
	try(Statement statement = connection.createStatement()){
        try(ResultSet re = statement.executeQuery("SELECT * FROM user")){
            while(re.next()){
                User user = new User();
            }
        }
	}
}
```