# 数据交互层所有API函数

## User
### 用户字段
- int id -> 用户ID
- QString name -> 用户名
- QString password -> 用户密码
- bool authority -> 用户权限，true为管理员，false为普通用户

### 相关函数

1. 添加用户
```c++
// 如果添加失败返回值为-1， 添加成功返回新增用户ID
int addUser(User user);

// ID由数据库自动生成, 增加用户只能增加普通用户, 因此ID和Authority字段会被忽略掉
User user = User(0, "name", "password", true/false);
int temp = dao->addUser(user);
if (temp == -1) {
	// 添加失败 ....
} else {
	// 添加成功 ....
}

```

2. 删除用户
```c++
// 删除成功返回true，否则返回false
bool deleteUser(int user);

// ID可以通过输入框获取也可以通过表格获取
int id = this.ui.QLineedit.text.toInt();
bool temp = dao->deleteUser(id);
if (temp) {
	// 删除成功
} else {
	// 删除失败
}
```

3. 更新用户
```c++
// 更新成功返回true, 否则返回false
bool updateUser(User user);

// ID可以通过输入框获取也可以通过表格获取
int id = this.ui.QLineedit.text.toInt();
// 获取需要修改的用户
User user = dao->getUserById(id);
// 更改用户的部分字段，但最好不要修改ID和权限
user.name = "";
user.password = "";
bool temp = dao->updateUser(user);
if (temp) {
	// 删除成功
} else {
	// 删除失败
}

```

4. 根据ID查找用户

```c++
// 根据ID查找用户，查找到了用户的ID为正数，没有查找到用户ID必为-1
User getUserById(int id);

// ID可以通过输入框获取也可以通过表格获取
int id = this.ui.QLineedit.text.toInt();
User user = dao->getUserById(id);

ui->tableWidget->setRowCount(1);

ui->tableWidget->setItem(0, 0, new QTableWidgetItem(QString::number(user.id)));

ui->tableWidget->setItem(0, 1, new QTableWidgetItem(user.name));

ui->tableWidget->setItem(0, 2, new QTableWidgetItem(user.password));

ui->tableWidget->setItem(0, 3, new QTableWidgetItem(user.authority));

```

5. 根据Name查找用户

```c++
// 根据Name查找用户，返回一个User列表，遍历列表即可获得所有用户
QList<User> getUsersByName(QString name);


QList<User> userList = dao->getUsersByName("name");

ui->tableWidget->setRowCount(userList.length());
for (int i = 0; i < userList.length(); i++) {
	User user = userList.at(i);
	ui->tableWidget->setItem(i, 0, new QTableWidgetItem(QString::number(user.id)));

ui->tableWidget->setItem(i, 1, new QTableWidgetItem(user.name));

ui->tableWidget->setItem(i, 2, new QTableWidgetItem(user.password));

ui->tableWidget->setItem(i, 3, new QTableWidgetItem(user.authority));
}

```

6. 查找所有用户

```c++
QList<User> getAllUsers();


QList<User> userList = dao->getAllUsers();

ui->tableWidget->setRowCount(userList.length());
for (int i = 0; i < userList.length(); i++) {
	User user = userList.at(i);
	ui->tableWidget->setItem(i, 0, new QTableWidgetItem(QString::number(user.id)));

ui->tableWidget->setItem(i, 1, new QTableWidgetItem(user.name));

ui->tableWidget->setItem(i, 2, new QTableWidgetItem(user.password));

ui->tableWidget->setItem(i, 3, new QTableWidgetItem(user.authority));
}

```

## Product
### 商品字段
- int id -> 商品ID
- QString name -> 商品名
- int type -> 商品种类
- float selling_price -> 商品售价
- float purchase_price -> 商品进价
- int stock -> 商品库存
- float discount -> 商品折扣
- int supplier_id -> 商品对应供货商ID

### 相关函数

1. 添加商品
```c++
// 如果添加失败返回值为-1， 添加成功返回新增商品ID
int addProduct(Product product);

// ID由数据库自动生成, 增加用户只能增加普通用户, 因此ID和Authority字段会被忽略掉
Product product = User(0, type, name, selling_price, purchase_price, stock, discount, supplier_id);

int temp = dao->addProduct(product);
if (temp == -1) {
	// 添加失败 ....
} else {
	// 添加成功 ....
}

```

2. 删除商品

```c++

// 删除成功返回true，否则返回false

bool deleteProduct(int id);


// ID可以通过输入框获取也可以通过表格获取

int id = this.ui.QLineedit.text.toInt();

bool temp = dao->deleteProduct(id);

if (temp) {

 // 删除成功

} else {

 // 删除失败

}

```

  

3. 更新商品

```c++

// 更新成功返回true, 否则返回false

bool updateProduct(Product product);

  

// ID可以通过输入框获取也可以通过表格获取

int id = this.ui.QLineedit.text.toInt();

// 获取需要修改的用户

Product product = dao->getProductById(id);

// 更改商品的部分字段，但最好不要修改ID

product.name = "";


bool temp = dao->updateProduct(product);

if (temp) {

 // 删除成功

} else {

 // 删除失败

} 

```

  

4. 根据ID查找商品

  

```c++

// 根据ID查找用户，查找到了用户的ID为正数，没有查找到用户ID必为-1

Product getProductById(int id);

  

// ID可以通过输入框获取也可以通过表格获取

int id = this.ui.QLineedit.text.toInt();

Product product = dao->getProductById(id);

ui->tableWidget->setRowCount(1);

ui->tableWidget->setItem(0, 0, new QTableWidgetItem(QString::number(product.id)));

ui->tableWidget->setItem(0, 1, new QTableWidgetItem(product.name));
...

  

```

  

5. 根据Name查找商品

  

```c++

// 根据Name查找商品，返回一个Product列表，遍历列表即可获得所有商品

QList<Product> getProductsByName(QString name);

  
  

QList<Product> productList = dao->getProductsByName("name");

  

ui->tableWidget->setRowCount(productList.length());

for (int i = 0; i < productList.length(); i++) {

 Product product = productList.at(i);

 ui->tableWidget->setItem(i, 0, new QTableWidgetItem(QString::number(product.id)));

ui->tableWidget->setItem(i, 1, new QTableWidgetItem(product.name));
...

}

  

```

6. 根据Type查找商品

```c++
// 根据Name查找用户，返回一个Product列表，遍历列表即可获得所有用户

QList<Product> getProductsByType(int type);


QList<Product> productList = dao->getProductsByType(type);

  

ui->tableWidget->setRowCount(productList.length());

for (int i = 0; i < productList.length(); i++) {

 Product product = productList.at(i);

 ui->tableWidget->setItem(i, 0, new QTableWidgetItem(QString::number(product.id)));

ui->tableWidget->setItem(i, 1, new QTableWidgetItem(product.name));
...

}


```

7. 根据Supplier查找商品

```c++

QList<Product> getProductsBySupplier(int supplier_id);


QList<Product> productList = dao->getProductsBySupplier(type);

  

ui->tableWidget->setRowCount(productList.length());

for (int i = 0; i < productList.length(); i++) {

 Product product = productList.at(i);

 ui->tableWidget->setItem(i, 0, new QTableWidgetItem(QString::number(product.id)));

ui->tableWidget->setItem(i, 1, new QTableWidgetItem(product.name));
...

}

```

8. 查找所有商品

  

```c++

QList<Product> getAllProducts();

  
  

QList<Product> productList = dao->getAllProducts();

  

ui->tableWidget->setRowCount(productList.length());

for (int i = 0; i < productList.length(); i++) {
 Product product = productList.at(i);
 ui->tableWidget->setItem(i, 0, new QTableWidgetItem(QString::number(product.id)));
ui->tableWidget->setItem(i, 1, new QTableWidgetItem(product.name));
ui->tableWidget->setItem(i, 2, new QTableWidgetItem(product.password));
ui->tableWidget->setItem(i, 3, new QTableWidgetItem(product.authority));
}
```

## Supplier

### 相关字段

- int id -> 供应商ID
- QString name -> 供应商名字


### 相关函数

1. 添加供应商
```c++ 
int addSupplier(Supplier supplier);
```

2. 删除供应商
```c++
bool deleteSupplierById(int id);
```

3. 更新供应商

```c++
bool updateSupplier(Supplier supplier);
```

4. 根据ID查找供应商

```c++
Supplier getSupplierById(int id);
```

5. 根据名字查找供应商


```c++
QList<Supplier> getSuppliersByName(QString name);
```

6. 查找所有供应商

```c++
QList<Supplier> getAllSuppliers();
```

```ad-note
函数用法与前文相似，不过多赘述了，可以仿照前文来写
```

## Order
### 相关字段
- int id -> 订单ID
- tm date -> 订单创建日期
- int count -> 下单商品数量
- int product_id -> 下单商品ID
- int user_id -> 下单用户ID


### 相关函数
1. 创建新订单
```c++
int addOrder(Order order);

  

 tm t;
 t.tm_year = 2021;
 t.tm_mon = 11;
 t.tm_mday = 22;
 dao->addOrder(Order(0, t,3, 1, 3));
```
2. 根据ID查找订单
```c++
  

Order getOrderById(int id);


 Order o = dao->getOrderById(3);
 qDebug() << o.product_id;
```
3. 查找所有订单
```c++
  

QList<Order> getAllOrders();

  

 QList<Order> t13 = dao->getAllOrders();
 for(Order o : t13) {
 qDebug() << o.count;
 }
```
4. 根据用户ID查找订单
```c++
  

QList<Order> getOrdersByUserId(int uid);


 QList<Order> t14 = dao->getOrdersByUserId(5);
 for(Order o : t14) {
 qDebug() << o.count;
 }
```
5. 根据商品ID查找订单
```c++
  

QList<Order> getOrdersByProductId(int pid);


 QList<Order> t15 = dao->getOrdersByProductId(3);
 for(Order o : t15) {
 qDebug() << o.id;
 }
```
6. 根据商品ID以及时间段查找订单
```c++
  

QList<Order> getOrdersByTimeSegmentAndProductId(tm t1, tm t2, int pid);

  

 tm t1;
 t1.tm_year = 2021;
 t1.tm_mon = 1;
 t1.tm_mday = 16;
 tm t2;
 t2.tm_year = 2022;
 t2.tm_mon = 1;
 t2.tm_mday = 22;
 QList<Order> t16 = dao->getOrdersByTimeSegmentAndProductId(t1, t2, 3);
 for(Order o : t16) {
 qDebug() << o.id;
 }
```
7. 根据用户ID以及时间段查找订单
```c++
  

QList<Order> getOrdersByTimeSegmentAndUserId(tm t1, tm t2, int uid);

  
 tm t3;
 t3.tm_year = 2021;
 t3.tm_mon = 1;
 t3.tm_mday = 18;
 tm t4;
 t4.tm_year = 2022;
 t4.tm_mon = 1;
 t4.tm_mday = 18;
 QList<Order> t17 = dao->getOrdersByTimeSegmentAndUserId(t3, t4);
 for(Order o : t17) {
 qDebug() << o.id;
 }
```
8. 根据时间段查找订单
```c++
  

QList<Order> getOrderByTimeSegment(tm t1, tm t2);


 tm t3;
 t3.tm_year = 2021;
 t3.tm_mon = 1;
 t3.tm_mday = 18;
 tm t4;
 t4.tm_year = 2022;
 t4.tm_mon = 1;
 t4.tm_mday = 18;
 QList<Order> t17 = dao->getOrderByTimeSegment(t3, t4);
 for(Order o : t17) {
 qDebug() << o.id;
 }
```