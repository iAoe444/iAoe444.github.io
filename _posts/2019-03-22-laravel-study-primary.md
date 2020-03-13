---
title: Laravel学习-基础篇
categories:
- note
tags:
- laravel
---

> 学习教程： [轻松学会Laravel基础视频教程-慕课网](https://www.imooc.com/learn/697)，[《Laravel 5.8 中文文档》](https://learnku.com/docs/laravel/5.8)，[轻松学会Laravel_Laravel框架教程-慕课网](https://www.imooc.com/learn/699)
>
> 书写于190322

## 1. 初识Laravel

### MVC简介

* Model是应用程序中用于处理应用程序数据逻辑的部分

  * 通常模型负责在数据库中存取数据

* View是应用程序中处理数据显示的部分

* Controller是应用程序处理用户交互的部分

  * 从Model获取数据，并输入到View中

    ![](https://assest.iaoe.xyz/img/006bBmqIgy1g5snhdhhdpj30bm05l3z7.jpg)

  * 接收View的用户操作，并响应相应操作

    ![](https://assest.iaoe.xyz/img/006bBmqIgy1g5snhj0qm1j30by05taav.jpg)

## 2. Laravel的安装及核心目录文件介绍

### 2.1 开发环境的搭建

Windows 下 Laravel 环境配置 | Laravel China 社区 - 高品质的 Laravel 开发者社区
https://learnku.com/articles/16868/laravel-environment-configuration-under-windows

### 2.3 核心目录文件介绍

* app 

  > 运行用的核心代码，自己的代码

  * http文件夹

    > Controllers

* bootstrap

  >  框架启动和自动加载配置

* config

  > 包含所有文件的配置文件

* databse

  > 数据库迁移与数据填充

* public

  > 项目的静态资源

* resource 

  >  视图和资源文件

  * views 视图文件夹

* storage 

  > 编译后的模板文件，session文件缓存，日志和其他框架层

* tests 

  > 单元测试

* vendor



## 3. Laravel的路由和MVC

### 3.1 路由

> routes/web.php

#### 1. 基本路由

get请求

```php
Route::get('test',function() {
   return 'test1';
});
```

post请求

```php
Route::post('test2',function(){
   return 'test2';
});
```

多请求路由

```php
Route::match(['get','post'],'test3',function (){
    return 'test3';
});
```

响应所有类型的请求，比如post，get之类的

```php
Route::any('test4',function(){
   return 'test4'; 
});
```

#### 2. 传递路由参数

```php
Route::get('user/{id}',function ($id){
    return 'User-'.$id;
});
Route::get('user/{name?}', function($name = null) {
    return 'User'.$name;
})->where('name', '[A-Za-z]+'); // 后面可以跟正则表达式验证这个参数
Route::get('user/{id}/{name?}', function($id, $name = null) {
    return 'User'.$name.$id;
})->where(['id' => '[0-9]+', 'name' => '[A-Za-z]+']); // 验证多参数
```

#### 3. 路由别名

```php
// 给路由给别名，这个别名可以在路由、控制器中用，另外如果以后想改url，有了别名，那么其他地方就不用改了
Route::get('user/center', ['as' => 'center', function() {
    return 'center';
}]); 
```

#### 4. 路由群组

```php
Route::group(['prefix' => 'member'], funciton() {
    Route::get('user/center', ['as' => 'center', function() {
    return 'center';
}]); 
    Route::get('user/{id}/{name?}', function($id, $name = null) {
    return 'User'.$name.$id;
})->where(['id' => '[0-9]+', 'name' => '[A-Za-z]+']);
});
// 将两个路由放到了一个路由群组里面，并且给两个路由加了一个前缀，访问member/user/center或者member/usr/3/才能真正访问到上面的两个路由，就是说给两个路由的url前加了'member/'
```

#### 5. 路由中输出视图

```php
Route::get('view', function() {
    return view('Hello');
});
```



### 3.2 控制器

#### 1. 怎么新建一个控制器

新建`app/Controllers/*Controller.php`

```php
<?
    namespace App\Http\Cotrollers;//命名空间
    class MemberController extends Controller
    {
        public function info()
        {
            return 'member-info';
        }
    }
```



#### 2. 控制器和路由怎么关联

在路由里面

```php
Route::get('member/info','MemberController@info');
Route::get('member/info',['uses'=>'MemberController@info']);
```



#### 3. 关联控制器后，路由的特性怎么用





### 3.3 视图

#### 1. 怎么新建视图

在目录views/里面新建`member-info.php`

或者默认视图为`member-info.blade.php`

#### 2. 怎样输出视图

```php
return view('member-info');
return view('member/info'); //member目录下的info
return view('member/info',['name'=>'sean']);//传参
```

如果传参后，视图要怎么查看呢

```php
{{$name}}   //这样就可以了
```



### 3.4 模型

#### 1. 怎样新建模型

在app下新建Members.php

```php
<?php
    namespace App;
    use Illuminate\Database\Eloquent\Model;

    class Member extends Model
    {
        public static function getMember()
        {
            return 'Member is me';
        }   
    }
```

#### 2. 怎样使用模型

在控制器中调用

```php
Member::getMember();
```

## 4. 数据库操作之—— DB facade

> Laravel中提供DB facade(原始查找)，查询构造器和Eloquent ORM三种操作数据库方式

### 1. 新建数据表与连接数据库

* 到phpmyadmin的网址下，去执行下面的语句

![](https://assest.iaoe.xyz/img/006bBmqIgy1g5snhty6qrj30gs06uq50.jpg)

* 连接数据库

  * 到`.env`文件改DB_HOST, DB_DATABASE, DB_USERNAME, DB_PASSWORD

* 查看数据库
  * `localhost/phpmyadmin`

### 2. 使用DB facade实现CURD

> 使用最原始的SQL语句来进行数据库操作

```php
//查找
$student = DB::select('select * from student where id > ?',[1001]); //返回一个数组
dd($student); //返回一个好看的数组方式

//新增
$bool = $insert = DB:insert('insert into student(name,age) values(?,?)',
                   ['me',18]); //返回一个布尔值

//修改
$num = DB::update('update student set age = ? where name = ?',
          [20, ' you']);//返回影响的行数

//删除
$num = DB:delete('delete from student where id >?',[1001]); //返回影响的行数
```

## 5. 数据库操作之——查询构造器

### 1.查询构造器简介及新增数据

* 简介
  * Laravel查询构造器（query builder）提供方便、流畅的接口，用来建立及执行数据库查找语法
  * 使用PDO参数绑定，以保护应用程序免于SQL注入因此传入的参数不需额外转义特殊字符
  * 基本可以满足所有的数据库操作，而且在所有支持的数据库系统上都可以执行

* 新增数据

  ```php
  public function query()
  {
      $bool = DB::table('student')->insert(
          ['name' => 'imooc','age' => 18]
      );
      
      //想获得插入数据返回的ID
      $id = DB::table('student')->insertGetId(
          ['name' => 'imooc','age' => 18]
      );
      
      //插入多条数据
      $id = DB::table('student')->insertGetId([
          ['name' => 'imooc','age' => 18]，
          ['name' => 'imooc2','age' => 21]
      ]);
  }
  ```

### 2.使用查询构造器更新数据

* 更新指定的内容

  ```php
  //更新id为12的项目的年龄为30
  $num = DB::table('student')
      ->where('id',12)
      ->update(['age'=>30]);
  ```

* 自增和自减

  > 指的是某个项的属性增加一个固定的值

  ```php
  $num = DB::table('student')->increment('age',3);//所有人的年龄增加3，默认为1
  $num = DB::table('student')->decrement('age',3);//所有人的年龄减少3，默认为1
  //返回值为影响的行数
  ```

  * 精准确定一个要自增减的实例

    ```php
    $num = DB::table('student')
        ->where('id',12)
        ->decrement('age',3); //所有人的年龄减少3，默认为1
    ```

  * 精准确定一个要自增减的实例，再修改其他字段

    ```php
    $num = DB::table('student')
        ->where('id',12)
        ->decrement('age',3,['name'=>'me']); //所有人的年龄减少3，默认为1
    ```

### 3.使用查询构造器删除数据

* **delete**方法

  ```php
  //删除整个表
  $num = DD::table('student')->delete();
  
  //精准删除
  $num = DB::table('student')
      ->where('id',15)
      ->delete();
  
  //删除id大于等于13的
  $num = DB::table('student')
      ->where('id','>=',13)
      ->delete();
  ```

* **truncate**方法

  > 用于整表删除

  ```php
  $num = DD::table('student')->truncate();
  ```

### 4.使用查询构造器查询数据

> 这些操作可以直接在控制器操作

* **get**方法

  > 获取表的所有数据

  ```php
  $stundets = DB::table('student')->get();
  dd($students);
  ```

* **first**方法

  > 获取查询结果的第一条数据

  ```php
  $stundets = DB::table('student')
      ->orderBy('id','desc')
      ->first();
  dd($students);
  ```

* **where**方法

  > 找到符合条件的数据

  ```php
  $stundets = DB::table('student')
      ->where('id','>=','13')
      ->get();
  dd($students);
  ```

  多个条件的写法

  ```php
  $stundets = DB::table('student')
      ->whereRaw('id>=? and age > ？','[1001,18]')
      ->get();
  ```

* **pluck**方法

  > 返回指定属性的字段

  ```php
  $names = DB::table('student')
      ->plunk('name');  //这里放回student表的所有name
  dd($names);
  ```

* **lists**方法

  > 返回指定属性的字段,可以指定返回数组的下标

  ```php
  $names = DB::table('student')
      ->lists('name'，'id'); //这里数组的下标为id
  dd($names);
  ```

* **select**方法

  > 返回指定的多个字段

  ```php
  $names = DB::table('student')
      ->select('name'，'id'，'age');  //只放回id，name，和age属性
    ->get();
  
  dd($names);
  ```

* **chunk**方法

  > 处理多条数据

  ```php
  echo '<pre>';
  DB::table('student')->clunk(2,function($students){
      var_dump($students);  //每次打印两条数据
      return false;         //只查一次。去掉全部打印
  })
  ```

### 5.查询构造器中的聚合函数

> 比如算最大值，算一共多少个，算总数

* **count**方法

  ```php
  $num = DB::table('student')->count(); //student表里面有多少个记录
  ```

* **max**方法

  ```php
  $max = DB::table('student')->max('age');  //找到年龄最大的人
  ```

* **min**方法

  ```php
  $min = DB::table('student')->min('age');  //找到年龄最小的人
  ```

* **avg**方法

  ```php
  $avg = DB::table('student')->avg('age');  //找到年龄的平均值
  ```

* **sum**方法

  ```php
  $sum = DB::table('student')->sum('age');  //找到年龄的总和
  ```

## 6. 数据库操作之——Eloquent ORM

> 这个Laravel操作数据库最方便的方式

### 1. Eloquent ORM简介、模型的建立及查询数据

* **简介**

  * Laravel 所自带的Eloquent ORM是一个优美、简洁的ActiveRecord 实现，用来实现数据库操作
  * 每个数据表都有一个与之相对应的“模型（Model）"用于和数据表交互

* **模型的建立**

  > 这里就要用到之前模型的相关知识点了，模型用于与数据库之间进行操作

  ```php
  //建立一个模型,在App目录下直接新建Student.php
  <?php
  namespace App;
  use Illuminate\Database\Eloquent\Model;
  class Student extends Model
  {
    //指定表名
    protected $table='student'；
    //指定id
      protected $primarykey='id'；       
  }
  ```

* **查询数据**

  ```php
  //查询所有数据
  public function orm1()
  {
      $Students = Student::all();       //返回一个集合
      dd($Students);
  }
  //查询ID等于13的
  public function orm2()
  {
      $Students = Student::find(13);        //查询id等于13的student
      dd($Students);
  }
  //查询ID等于13的
  public function orm3()
  {
      $Students = Student::findOrFail(1001);        //查询id等于13的student，没找到，抛出异常
      dd($Students);
  }
  //使用get，where，first,chunk等
  public function orm4()
  {
      $Students1 = Student::get();      //查询所有数据
      $Students2 = Student::where('id','>','1001')  //查询id>1001的数据
          ->orderBy('age','desc')
          ->first();
      Student::chunk(2,function($students3){    //两个两个查询数据
          var_dump($students3);
      })
  }
  //使用聚合函数
  public function orm5()
  {
      $num = Student::count();
      $max = Student::where('id','>',1001)->max('age'); //记得后面要变成->
  }
  ```

  

### 2. Eloquent ORM中新增数据、自定义时间戳及批量赋值的使用

* **通过模型新增数据**（涉及到自定义时间戳）

  ```php
  
  public function orm1()
  {
      //使用模型新增数据
      $student = new Student();
      $student->name = 'me';
      $student->age = 18;
      $bool = $studnet->save(); //返回是否成功
  }
  ```

  这里会自动维护created和updated字段，所以上述代码运行后，会发现会自动加上一个时间

  ![](https://assest.iaoe.xyz/img/006bBmqIgy1g5sni0llc7j30fg04r0vg.jpg)

  * 如果不想自动添加时间戳，就打开之前模型，进行设置，新增

  ```php
  public $timestamps = false;
  ```

  * 如果想自定义设置时间戳，那么就再新增一个函数

  ```php
  public $timestamps = true;
  protected function getDateFormat()
  {
      return time();    //自定义时间戳
  }
  ```

  ​ 数据库中显示的效果是这样的

  ​ `190359205604`

  ​ 而获取到的效果时这样的

  ​ `2019-03-29 20:56:04`

  * 如果不想获取的时候那样显示，直接用时间戳，那么就再在这基础上，新增

    ```php
    protected function asDateTime($val)
    {
        return $val;
    }
    ```

    效果是这样的，获取到的直接就是一个时间戳，

    `190359205604`

    这样做有什么好处呢，就是可以通过这个时间戳来格式化这个代码

    ```php
    echo date('Y-m-d H:i:s',time);
    ```

* **使用模型Create方法新增数据**（涉及到批量赋值）

  ```php
  $student = Student::create(
      ['name'=>'imooc','age'=>18]
  );
  ```

  这样做是不行的，因为不允许`批量赋值`，需要改模型

  ```php
  protected $fillable = ['name','age']; //这里填写允许批量赋值的字段
  ```

  `不允许批量赋值的字段`，这样填写

  ```php
  protected $guarded = [];  //里面些不允许的字段
  ```

  * **firstOrCreate**方法

    > 如果数据库已经有该元素，那么返回元素，如果没有，则创建

    ```php
    $student = Student::firstOrCreate(
        ['name' => 'imooc']
    );
    //返回新增的数据，或者查找的数据
    ```

  * **firstOrNew**方法

    > 如果数据库已经有该元素，那么返回元素，如果没有，则需要save即可

    ```php
    $student = Student::firstOrNew(
        ['name' => 'imooc']
    );
    $bool = student->save();
    ```

### 3. 使用Eloquent ORM修改数据

* 通过模型更新数据

  ```php
  $student = Student::find(1021);
  $student->name = 'kitty';
  $bool = $student->save();
  ```

* 结合查询语句，批量更新

  ```php
  $num = Student::where('id','>',1029)->update(
      ['age'=>41]
  );    //返回受影响的行数
  ```

### 4. 使用Eloquent ORM删除数据

* 通过模型删除

  ```php
  $student = Student::find(1021);
  $bool = $student->delete();
  ```

* 通过主键删除

  ```php
  $num = Student::destroy(1020，1018);   //参数是主键的id
  //或者
  $num = Student::destroy([1020，1018]); //参数是主键的id
  ```

* 根据指定条件删除

  ```php
  $num = Student::where('id','>',1004)->delete();//num是影响的行数
  ```

## 7. Blade模板引擎

### 1. Blade模板引擎简介及模板继承的使用

> 由于制作网页的过程中很多页面都是一样的，这样就需要用到模板引擎了

* **简介**

  * Blade 是Laravel提供的一个既简单又强大的模板引擎
  * 和其他流行的PHP模板引擎不一样，Blade并不限制你在视图（view）中使用原生PHP代码
  * 所有Blade 视图页面都将被编译成原生PHP代码并缓存起来，除非你的模板文件被修改了，否则不会重新编译

* **模板继承的使用**

  * 首先先在`resources/views`里面新建一个父模板文件`layouts.blade.php`

    ```php
    //这个是父模板
    //resources/views/layout.blade.php
    <title>轻松学习Laravel - @yield('title')</title>
    
    <div class="header">
        @section('header')
        我是父模板的头部
        @show
    </div>
    
    <div class="content">
        @yeild('content','我是父模板的主要内容区域')
    </div>
    
    <div class="footer">
        @section('footer')
        我是父模板的底部
        @show
    </div>
    ```

    * 这里的`yeild`是不可扩展，如果没定义，就用默认内容
    * 这里的`section`可以扩展，可替代，比如使用`@parent`就可以父元素和子元素一起显示

  * 其次在`resource/views/student`中新建一个子模版文件`child.blade.php`

    ```php
    //resorce/views/student/child.blade.php
    @extends('layouts')     //如果父模板在views/member中，那么就写member.layouts
        
    @section('header')
        我是子模版的头部
    @stop
        
    //这里定义yeild,我们定义一个section，如果不定义的话就会呈现'我是父模板的主要内容区域'  
    @section('content')
        我是子模板的主要内容区域
    @stop
        
    @section('footer')
        @parent             //这样就显示效果是父模板中的“我是父模板的底部”和我是子模版的头部都会呈现
        我是子模版的头部
    @stop
    ```

  * 写好这些后就需要配置控制器了

    ```php
    // App/Http/Controllers/StudentController.php
    <?php
    
    namespace App\Http\Controllers;
    
    
    use App\Student;
    
    class StudentController
    {
        public function view() {
            return view('student.child'); // 访问views目录底下的student/child.blade.php
        }
    }
    ```

  * 最后还需要配置一下路由就ok了

### 2. 基础语法及include的使用

* **模板中输出PHP变量**

  * 首先设置模板

    ```php
    // App/Http/Controllers/StudentController.php
    <?php
    
    namespace App\Http\Controllers;
    
    
    use App\Student;
    
    class StudentController
    {
        public function view() {
            $name = 'sean';
            return view('student.child',['name' => $name]); //传入变量
        }
    }
    ```

  * 在模板中设置变量

    ```php
    //resources/views/layout.blade.php
    @section('content')
        我是子模板的主要内容区域
        <p>{{$name}}</p>
    @stop
    ```

* **模板中调用PHP代码**

  ```php
  //resources/views/layout.blade.php
  @section('content')
      我是子模板的主要内容区域
      <p>{{ time() }}</p>
      <p>{{ date('Y-m-d H:i:s', time()) }}</p>
      <p>{{ var_dump($name) }}</p>
  @stop
  ```

* **原样输出**

  > 我就想输出前面的{{ time() }},而不是调用这个函数

  ```php
  //resources/views/layout.blade.php
  @section('content')
      我是子模板的主要内容区域
      <p>@{{ time() }}</p>
  @stop
  ```

* **模板中的注释**

  ```php
  //这个是html的注释，在输出的网页源码中能看到
  <!-- 我是html注释 -->
  //这是模板中的注释，在输出的网页源码中看不到
  {{-- 我是模板中的注释 --}}
  ```

* **引入子视图 include的使用**

  * 首先，创建一个子视图

    ```php
    //resources/views/student/childView.blade.php
    <p> 我是子视图，这是我被传的参数{{ $message }} </p>
    ```

  * 接着在要显示的模板中设置

    ```php
    //resources/views/layout.blade.php
    @section('content')
        我是子模板的主要内容区域
        @include('student.childView',['message' => '{{ $message }}' ])
    @stop
    ```

### 3. 流程控制

> `if`，`unless`，`for`，`foreach`

```php
//resources/views/layout.blade.php
@section('content')
    我是子模板的主要内容区域
    
    {{-- if --}}
    @if ($name == 'me')
        我就是我
    @elseif ($name == 'yanhuo')
        不一样的烟火
    @else
        下一句怎么唱来着？
    @endif
    
    //if中可以可以用php函数的
    @if (date('Y-m-d H:i:s', time())=='2019-04-01 00:00:00')
        今天我女装
    
    {{-- unless --}}
    $unless($name !='me' )
        我就是我
    $endunless
    
    {{-- for --}}
    @for ($i=0;$i<10;$i++)
        <p>{{ $i }}</p>
    @endfor
        
    {{-- foreach --}}
    $foreach($students as $student)
        <p>{{ $student->name }}</p>
    $endforeach
     
    //效果和foreach一样，但是如果空，输出null
    $forelse($students as $student)
        <p>{{ $student->name }}</p>
    $empty
        <p>null</p>
    $endforelse
@stop
```

### 4. 模板中URL

> 如果要用到url的话是不是觉得很麻烦，那么就需要模板中的url了

```php
<a href="{{ url('路由名字') }}"> 使用路由名 </a>
<a href="{{ action('StudentController@urlTest') }}"> 使用控制器的名字 </a>
<a href="{{ route('路由别名') }}"> 使用路由别名 </a>
```

## 8. Controller

### 1.1 Controller之Request

> Laravel中的请求使用的是symfony/http-foundation组件
>
> 请求里面存放了 $\_GET，$\_POST，$\_COOKIE，$\_FILES，$\_SERVER等数据

- **获取请求中的值**

  ```php
  <?php
  
  namespace App\Http\Controllers;
  
  //!dw(ﾟДﾟ)w 注意，这里要选择这种Request的包
  use Illuminate\Http\Request;
  
  class Controller extends Controller
  {
      public function request(Request $request)
      {
          /* 获取请求中的值 */
          //下面的一行代码应用于这种类型的传参'/?name=me'
        echo $request->input('name');   //返回一个参数就是me
          
          /* 设置默认值 */
          echo $request->input('name','未知');        //这里是一个默认的值，如果没用传参，则$request的值是未知
          
          /* 判断是否有该参数 */
          if($request->has('name')){
              echo $request->input('name');
          }else{
            echo '无该参数';
          }
          
          /* 获取request的所有参数 */
          $res = $request->all();
      }
  }
  ```

- 判断请求类型

  ```php
  <?php
  
  namespace App\Http\Controllers;
  
  //!dw(ﾟДﾟ)w 注意，这里要选择这种Request的包
  use Illuminate\Http\Request;
  
  class Controller extends Controller
  {
      public function request(Request $request)
      {
          /* 获取请求类型 */
          echo $request->method();
          
          /* 判断是否是指定的请求 */
          if($request->isMethod('POST')){
              echo '是POSSSSSST啊';
          } else{
              echo '不是';
          }
          
          /* 判断是否是Ajax请求 */
          if($request->ajax()){
              echo '是Ajaaaaaax啊';
          } else{
              echo '不是';
          }
          
          /* 判断请求路径是否是规定的请求路径 */
          $res = $request->is('student/*');
          
          /* 获取当前的url */
          $res = $request->url();
      }
  }
  ```

### 1.2. Controller之Session

> 由于HTTP协定是无状态（Stateless）的，所以session 提供一种保存用户数据的方法
>
> Laravel 支持了多种 session 后端驱动，并提供清楚、统一的API。也内置支持如 Memcached、Redis和数据库的后端驱动。默认使用"file"的Session驱动
>
> session 的配置文件配置在`config/session.php`中

#### 1.2.1. Session的三种使用方式

- 使用HTTP request类的session()方法使用session

  > 这里先注册一个路由调用putSession方法，再注册一个路由调用getSession方法

  ```php
  <?php
  
  namespace App\Http\Controllers;
  
  //!dw(ﾟДﾟ)w 注意，这里要选择这种Request的包
  use Illuminate\Http\Request;
  
  class Controller extends Controller
  {
      public function putSession(Request $request)
      {
          /* Http requst session()放置session */
            $request->session()->put('key1','value1');
      }
      
      public function getSession(Request $request)
      {
          /* Http requst session()获取session */
            echo $request->session()->get('key1');
          /* 使用默认值取session */
          echo $request->session()->get('key1','default');
      }
  }
  ```

- 使用session()辅助函数

  ```php
  <?php
  
  namespace App\Http\Controllers;
  
  //!dw(ﾟДﾟ)w 注意，这里要选择这种Request的包
  use Illuminate\Http\Request;
  
  class Controller extends Controller
  {
      /* 使用session()辅助函数 */
      public function session(Request $request)
      {
        session()->put('key2','value2');   
      }
      
      public function getSession(Request $request)
      {
            echo session()->get('key2');
      }
  }
  ```

- 使用Session facade（Session类）

  ```php
  <?php
  
  namespace App\Http\Controllers;
  
  //!dw(ﾟДﾟ)w 注意，这里要选择这种Request的包
  use Illuminate\Http\Request;
  //!!!!dw(ﾟДﾟ)w 注意，这里要选择这种Session的包
  use Illuminate\Support\Facades\Session;
  
  class Controller extends Controller
  {
      /* 使用session()辅助函数 */
      public function session(Request $request)
      {
        session::put('key3','value3');   
      }
      
      public function getSession(Request $request)
      {
            echo session::get('key3');
      }
  }
  ```

#### 1.2.2.  在session里面存储数组

> 下面的这些代码都用一个存的方式，一个取的方式写，是伪代码，具体实现看上面的代码就行

```php
Session::put(['key4' => 'value4']]);
Session::get('key4');
```

#### 1.2.3. 把数据放在Session数组中

> 不断在session里面存储数据

```php
Session::push('name','me');
Session::push('name','you');

Session::get('name','default'); //返回一个数组，显示的是['name’[0]=>'me','name‘[1]=>'you']
```

这里的get是取完数据不删除，使用pull可以删除数据

```php
Session::push('name','me');
Session::push('name','you');

Session::pull('name','default');    //返回一个数组，显示的是['name’[0]=>'me','name‘[1]=>'you']
Session::pull('name','default');    //再次使用，返回default
```

#### 1.2.4. 使用Session所有的值

```php
Session::all();
```

#### 1.2.5. 判断Session某个key是否存在

```php
Session::has('name');
```

#### 1.2.6. 删除Session中的某个key

```php
Session::forget('name');
```

#### 1.2.7. 删除Session所有数据

```php
Session::flush();
```

#### 1.2.8. 一次性的Session

> 第一次访问是存在的，第二次访问不存在

```php
Session::flash('key-flash','value-flash');
```

### 1.3. Controller之Response

- **响应的常见类型**

  - 字符串 

  - 视图

  - Json

    ```php
    <?php
    
    namespace App\Http\Controllers;
    
    class Controller extends Controller
    {
        /* 响应json */
        public function response()
        {
            $data = [
                'errCode' => 0,
                'errMsg' => 'success',
                'data' => 'me'
            ];
            return reponse()->json($data);
        }
    
    }
    ```

  - 重定向

    ```php
    <?php
    
    namespace App\Http\Controllers;
    
    class Controller extends Controller
    {
        /* 重定向 */
        public function response()
        {
            //这里路由名字是自己设置的路由名字，重定向的话有message里的hello的session发出，是一个flash的session
            return redirect('路由名字')->with('message','hello');
            
            //第二种重定向方法action()
            return redirect()->action('StudentController@session2')->with('message','hello');
            
            //第三种重定向方法route(),里面的是路由别名
            return redirect()->route('session2');
            
            //第四种重定向方法,返回上一层页面
            return redirect()->back();
        }
    
    }
    ```

### 1.4. Controller之Middleware

> Laravel中间件提供一个方便的机制来过滤进入应用程序的HTTP请求，类似于javaweb的fitter

现在有个场景是这样的，有一个活动，在指定日期后开始，如果活动没开始只能访问宣传页面

- 开始前的准备

  - 首先是在Controller里面建一个活动页面，一个宣传页面

    ```php
    <?php
    
    namespace App\Http\Controllers;
    
    class Controller extends Controller
    {
        /* 宣传页面 */
        public function activity0()
        {
            return '尽情期待，博主女装真人秀ヾ(•ω•`)o';
        }
        
        /* 活动页面 */
        public function activity1()
        {
            return '(⊙﹏⊙)没想到你真来了';
        }
    }
    ```

  - 新建路由

    ```php
    Route::any('activity0',['uses'=>'Controller@activity0']);
    Route::any('activity1',['uses'=>'Controller@activity1']);
    ```

- 新建中间件

  ```php
  //app\Http\Middleware\Activity.php
  <?php
  
  //命名空间
  namespace App\Http\Middleware;
  use Closure;
  
  //新建一个类
  class Activity
  {
      public function handle($request, Closure $next)
      {
          if(time()!=strtotime('2019-04-01')){
              return redirect('activity0');
          }
          
          //这个函数的返回的返回结果是response，
          return $next($request);
      }
  }
  ```

- 注册中间件

  > 这里定义的是为特定路由分配的中间件，而另外一个是全局中间件，可以在你应用的每个 HTTP 请求期间运行，只需在 `app/Http/Kernel.php` 类中的 `$middleware` 属性里列出这个中间件类 

  ```php
  //app\Http\Request\Kernel.php
  protected $routeMiddleware = [
      'activity(中间件命名)' => \App\Http\Middleware\Activity::class,
  ];
  ```

- 使用中间件

  > 接下来就要改一下路由了，把之前的改下

  ```php
  //第一种方式
  Route::any('activity0',['uses'=>'Controller@activity0']);
  Route::any('activity1',['uses'=>'Controller@activity1'])->middleware('activity');
  
  //第二种方式
  Route::any('activity0',['uses'=>'Controller@activity0']);
  Route::group(['middleware' => ['activity']] ,function(){
    Route::any('activity1',['uses'=>'Controller@activity1']);  
  })
  ```

- 中间件的前置和后置操作

  > 操作return reponse前执行的是前置操作，在return reponse后执行的是后置操作

��

