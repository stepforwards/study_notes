---
title: Spring03
tags: Spring,框架,Java
grammar_cjkRuby: true
---

## spring整合JDBC

> spring中提供了一个可以操作数据库的对象,这个对象封装了jdbc技术,这个对象叫做JDBCTemplate(JDBC模版对象)

### 演示JDBCTemplate如何进行数据库的操作

- 导包4+2+1(aop)+1(test)+JDBC驱动+c3p0连接池+spring-jdbc(spring包中)+spring-tx(spring包中)
- 书写Dao层接口
- 书写Dao的实现类
- 编写配置文件
- 书写测试类

####  书写Dao层接口

``` stylus
public interface UserDao {

	void insertUser(User u);
	void deleteUser(User u);
	void updateUser(User u);
	List<User> selectAllUser();
}

```
#### 书写Dao的实现类

``` stylus
public class UserDaoImpl implements UserDao{

	@Autowired
	private JdbcTemplate jt;

	public void setJt(JdbcTemplate jt) {
		this.jt = jt;
	}
	// 插入操作
	@Override
	public void insertUser(User u) {
		String sql = "insert into t_user values (null,?,?)";
		jt.update(sql, u.getUsername(),u.getPassword());
	}
	// 查询操作
	@Override
	public List<User> selectAllUser() {
		String sql = "select * from t_user";
		List<User> users = jt.query(sql, new RowMapper<User>(){

			@Override
			public User mapRow(ResultSet rs, int index) throws SQLException {
				User user = new User();
				user.setId(rs.getInt("id"));
				user.setPassword(rs.getString("password"));
				user.setUsername(rs.getString("username"));
				return user;
			}
			
		});

		return users;
	}

	// 删除操作
	@Override
	public void deleteUser(User u) {
		String sql = "delete from t_user where id = ?";
		jt.update(sql, u.getId());
	}
	// 更新操作
	@Override
	public void updateUser(User u) {
		String sql = "update t_user set username = ? where id = ?";
		jt.update(sql, u.getUsername(),u.getId());
	}
}

```


#### 书写配置文件


