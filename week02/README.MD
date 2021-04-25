问题：
1. 我们在数据库操作的时候，比如 dao 层中当遇到一个 sql.ErrNoRows 的时候，是否应该 Wrap 这个 error，抛给上层。为什么，应该怎么做请写出代码？

`sql.ErrNoRows` 不需要 Wrap 这个 error，抛给上级。DAO的作用是封装数据库的访问，不涉及业务逻辑，ErrNoRows直接返回nil。

```go
func queryUser() (users, error){
	sqlStr := "select id, name, age from users where id=?"

	var u user
	err := db.QueryRow(sqlStr, id).Scan(&u.id, &u.name, &u.age)

	if err != nil && err != sql.ErrNoRows {
		return member, err
	}

	if err == sql.ErrNoRows {
		return member, nil
	}

	return u, nil
}
```