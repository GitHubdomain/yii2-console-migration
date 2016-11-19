
yii2使用migration备份和还原数据库
===========================
yii2使用migration备份和还原数据库，最初只想做一个在命令行中备份的功能，后来将类重组了，增加了其他扩展使用方法。


安装 Installation
------------

The preferred way to install this extension is through [composer](http://getcomposer.org/download/).

Either run

```
composer require --prefer-dist e282486518/yii2-console-migration "*"
```

or add

```
"e282486518/yii2-console-migration": "*"
```

to the require section of your `composer.json` file.


命令行中备份数据表：
-----

在```console\config\main.php```中添加  :

```php
'controllerMap' => [
    'migrates' => [
        'class' => 'e282486518\migration\ConsoleController',
    ],
],
```

在命令行中使用方式：
```
php ./yii migrates/create all #备份全部表
php ./yii migrates/create table1,table2,table3... #备份多张表
php ./yii migrates/create table1 #备份一张表

php ./yii migrates/up #恢复全部表
```

在后台中备份数据表：
-----
在后头的控制器中，例如```PublicController```中加入下面的代码：
```php
public function actions()
{
    return [
        'backup' => [
            'class' => 'e282486518\migration\WebAction',
            'returnFormat' => 'json',
            'migrationPath' => '@console/migrations'
        ]
    ];
}
```
在后台中发送一个ajax请求到```/admin/public/backup?tables=yii2_ad,yii2_admin```即可。

其他使用方法：
-----

对于想做更多扩展的朋友，可以直接继承```e282486518\migration\components\MigrateCreate```

或者使用一下代码：
```php
$migrate = Yii::createObject([
        'class' => 'e282486518\migration\components\MigrateCreate',
        'migrationPath' => $this->migrationPath
]);
$migrate->create($table);
```
