问题：
1. 我们在数据库操作的时候，比如 dao 层中当遇到一个 sql.ErrNoRows 的时候，是否应该 Wrap 这个 error，抛给上层。为什么，应该怎么做请写出代码？

`sql.ErrNoRows` 不需要 Wrap 这个 error，抛给上级。DAO的作用是封装数据库的访问，不涉及业务逻辑，错误直接返回。


func queryUser() (users, error){
	sqlStr := "select id, name, age from users where id=?"

	var u user
	err := db.QueryRow(sqlStr, id).Scan(&u.id, &u.name, &u.age)

	if err == sql.ErrNoRows {
		// fmt.Println("没找到记录")
		return u, nil
	}

	if err != nil {
		// fmt.Printf("%T, err:%v\n", err, err)
		return u, errors.WithMessage(err, fmt.Sprintf("test.users : 查询 id=%v 的记录出错 \n", id))
	}

	return u, nil
}
