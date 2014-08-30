#MYSQL C API Documentation
首先下载MySQL Connectors中的Connector/C，最新版为6.1.5             
我下载的是
```text
Linux - Generic (glibc 2.5) (x86, 64-bit), Compressed TAR Archive
(mysql-connector-c-6.1.5-linux-glibc2.5-x86_64.tar.gz)
```
下载地址: [http://dev.mysql.com/downloads/connector/c/](http://dev.mysql.com/downloads/connector/c/)
```bash
tar -zxvf mysql-connector-c-6.1.5-linux-glibc2.5-x86_64.tar.gz
cd mysql-connector-c-6.1.5-linux-glibc2.5-x86_64
mv lib /usr/lib/mysql
mv include /usr/include/mysql
echo -e "/usr/lib/mysql" > /etc/ld.so.conf.d/usr_lib_mysql.conf
/sbin/ldconfig
```
####MYSQL数据结构
```c
#include <mysql.h>

typedef struct st_mysql
{
  NET		net;			/* Communication parameters */
  unsigned char	*connector_fd;		/* ConnectorFd for SSL */
  char		*host,*user,*passwd,*unix_socket,*server_version,*host_info;
  char          *info, *db;
  struct charset_info_st *charset;
  MYSQL_FIELD	*fields;
  MEM_ROOT	field_alloc;
  my_ulonglong affected_rows;
  my_ulonglong insert_id;		/* id if insert on table with NEXTNR */
  my_ulonglong extra_info;		/* Not used */
  unsigned long thread_id;		/* Id for connection in server */
  unsigned long packet_length;
  unsigned int	port;
  unsigned long client_flag,server_capabilities;
  unsigned int	protocol_version;
  unsigned int	field_count;
  unsigned int 	server_status;
  unsigned int  server_language;
  unsigned int	warning_count;
  struct st_mysql_options options;
  enum mysql_status status;
  my_bool	free_me;		/* If free in mysql_close */
  my_bool	reconnect;		/* set to 1 if automatic reconnect */

  /* session-wide random string */
  char	        scramble[SCRAMBLE_LENGTH+1];
  my_bool unused1;
  void *unused2, *unused3, *unused4, *unused5;

  LIST  *stmts;                     /* list of all statements */
  const struct st_mysql_methods *methods;
  void *thd;
  /*
    Points to boolean flag in MYSQL_RES  or MYSQL_STMT. We set this flag 
    from mysql_stmt_close if close had to cancel result set of this object.
  */
  my_bool *unbuffered_fetch_owner;
  /* needed for embedded server - no net buffer to store the 'info' */
  char *info_buffer;
  void *extension;
} MYSQL;
```
####mysql_init
```c
#include <mysql.h>

MYSQL *		STDCALL mysql_init(MYSQL *mysql);
```
这个函数，用来分配或者初始化一个MYSQL对象，用于连接MYSQL服务端，如果传入的参数是NULL,它将自动为你分配一个MYSQL对象，但是我们最好不要使用NULL，因为如果多次不当调用mysql_close之后，会导致程序崩溃
```c
MYSQL *my = mysql_init(NULL);
```
```c
MYSQL my;
mysql_init(&my);
```
####mysql_real_connect
该函数用于连接MYSQL服务端，连接成功返回MYSQL指针，失败返回NULL，返回值和第一个参数的值相同
```c
MYSQL *		STDCALL mysql_real_connect(MYSQL *mysql, const char *host,
					   const char *user,
					   const char *passwd,
					   const char *db,
					   unsigned int port,
					   const char *unix_socket,
					   unsigned long clientflag);
```
`MYSQL *mysql`就是`mysql_init()`函数初始化后的结构指针              
`const char *host`主机名或IP地址,如果值为NULL或localhost,将被视为连接本地主机                  
`const char *user`用户名,如果值NULL或''空字符串，它就是系统登录名
`const char *passwd`密码，值NULL，会检查空密码的用户              
`const char *db`数据库名，值NULL，连接会将默认的数据库设为该值             
`unsigned int port`端口，通常3306         
`const char *unix_socket`连接类型，值NULL，表示通过host字段，自动判断,如果是本地的，也可以输入类似`/tmp/mysql.sock`              
`unsigned long clientflag`值通常为0               
####mysql_real_query
该函数用于执行SQL查询语句，查询成功返回0，失败返回非0
```c
int		STDCALL mysql_real_query(MYSQL *mysql, const char *q,
					unsigned long length);
```
`MYSQL *mysql`就是`mysql_real_connect()`函数连接成功的结构指针                    
`const char *q`SQL语句                    
`unsigned long length`SQL语句长度           
####MYSQL_RES
该结构用于保存SQL查询结果
```c
typedef struct st_mysql_res {
  my_ulonglong  row_count;
  MYSQL_FIELD	*fields;
  MYSQL_DATA	*data;
  MYSQL_ROWS	*data_cursor;
  unsigned long *lengths;		/* column lengths of current row */
  MYSQL		*handle;		/* for unbuffered reads */
  const struct st_mysql_methods *methods;
  MYSQL_ROW	row;			/* If unbuffered read */
  MYSQL_ROW	current_row;		/* buffer to current row */
  MEM_ROOT	field_alloc;
  unsigned int	field_count, current_field;
  my_bool	eof;			/* Used by mysql_fetch_row */
  /* mysql_stmt_close() had to cancel this result */
  my_bool       unbuffered_fetch_cancelled;  
  void *extension;
} MYSQL_RES;
```
####mysql_store_result
该函数返回查询SQL的结果集，成功返回MYSQL_RES指针，失败返回NULL
```c
MYSQL_RES *     STDCALL mysql_store_result(MYSQL *mysql);
```
该函数用于，从服务器获得查询返回的所有行，并将他们存储在客户端，保存在动态申请的MYSQL_RES数据结构中，返回该结构的指针  
####mysql_fetch_row
该函数是从MYSQL_RES结果集中，提取一行数据，如果返回NULL,表示数据提取完了
```c
MYSQL_ROW	STDCALL mysql_fetch_row(MYSQL_RES *result);
```
首先我们来看下`MYSQL_ROW`的定义，很明显就是一个指针的指针，也就是字符串数组
```c
typedef char **MYSQL_ROW;
```
