---

title: using-mysql-with-nodejs

---

## 安装mysql

在本地安装mysql

```shell
brew install mysql
```

启动mysql服务

```shell
brew services start mysql
```

设置安全选项

```shell
mysql_secure_installation
```

创建数据库

```mysql
create database mingda_thinking character set utf8mb4 collate utf8mb4_unicode_ci;
```

创建数据表

```sql
```

## 操作数据库

获取数据库配置

```javascript
const Conf = require('conf');

function getDatabaseConfig() {
  const config = new Conf();
  const databaseConfig = config.get('db');

  return databaseConfig;
}
```

创建数据库连接

```javascript
async function createConnection() {
  const databaseConfig = getDatabaseConfig();
  const connection = await mysql.createConnection(databaseConfig);
  return connection;
}
```

执行一条简单的SQL语句，执行完马上关闭

```javascript
async function executeSql(sql, params) {
  const connection = await createConnection();
  const result = await connection.execute(sql, params);
  await connection.end();

  return result;
}
```

再封装一下，如果是操作类的，返回成功或者失败

```javascript
async function execute(sql, params) {
  const [resultSetHeader] = await executeSql(sql, params);
  const { affectedRows } = resultSetHeader;

  return affectedRows > 0;
}
```

如果是查询类的，则返回查询结果

```javascript
async function query(sql, params) {
  const [rows] = await executeSql(sql, params);
  return rows;
}
```

按照分页获取列表类数据

```javascript
async function retrieveNotes(page = 1, pageSize = 10){
  const offset = ( page - 1 ) * pageSize;
  const sql = `SELECT * FROM notes LIMIT ${offset},${pageSize}`;
  const data = await db.query(sql);
  const meta = {page};

  return {
    data,
    meta
  }
}
```

新增一条记录

```javascript
async function createNote(content){
  const sql = `insert into notes (content) values (?)`;
  const params = [content];
  const succeed = await db.execute(sql, params);

  return { succeed };
}
```

修改一条记录的某个字段

```javascript
async function updateNote(id, content){
  const sql = `update notes set content = ? where id = ?`;
  const params = [content, id];
  const succeed = await db.execute(sql, params);

  return { succeed };
}
```

删除一条记录

```javascript
async function deleteNote(id){
  const sql = `delete from notes where id = ?`;
  const params = [id];
  const succeed = await db.execute(sql, params);

  return { succeed };
}
```
