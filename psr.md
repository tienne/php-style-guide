# 문서 소개

- 해당 가이드는 `PSR-1` / `PSR-2` 를 기반으로하는 일반적인 코딩 가이드입니다.

각 제안 요소들은 `MUST`, `MUST NOT`, `REQUIRED`, `SHALL`, `SHALL NOT`, `SHOULD`, `SHOULD NOT`, 
`RECOMMENDED`, `MAY`, and `OPTIONAL` 키워드를 이용하여 분류하고 있습니다. 

`MUST(반드시)`가 붙은 요소들은 반드시 지켜야 하는 것들이고, `OPTIONAL`은 선택사항입니다. 
키워드들에 대한 자세한 내용은 [rfc2119](http://www.ietf.org/rfc/rfc2119.txt)를 참고하시기 바랍니다.

# 참고 자료

- [PSR-1](http://www.php-fig.org/psr/psr-1/)
- [PSR-2](http://www.php-fig.org/psr/psr-2/)
- [PHP The Right Way](http://modernpug.github.io/php-the-right-way/)
- [PHP 언어의 잘못된 설계](https://github.com/tienne/php-a-fractal-of-bad-design)

# 목차

- [1. 일반적인 내용](#1-일반적인-내용)
    - [PHP 구문 관련](#11-php-구문-관련)
    - [FILE 관련](#12-file-관련)
    - [Line 관련](#13-line-관련)
    - [들여쓰기](#14-들여쓰기)
    - [PHP keyword](#15-php-keyword-와-truefalsenull)
    - [비교연산자](#16-비교연산자)
    - [삼항연산자](#17-삼항연산자)
- [2. Namespace 와 Use 선언](#2-namespace-와-use-선언-관련)
- [3. 클래스, 속성, 메서드](#3-클래스-속성-메서드)
    - [class 정의](#31-class-정의구문)
    - [Extends 와 Implements](#32-extends-와-implements)
    - [클래스 상수](#33-클래스-상수const)
    - [클래스 Properties](#34-클래스-properties멤버-변수)
    - [클래스 메서드](#35-클래스-메서드)
    - [함수 혹은 메서드 호출](#36-함수-혹은-메서드-호출)
- [4. 제어문](#4-제어문)
    - [if, elseif, else](#41-if-elseif-else)
    - [switch, case](#42-switch-case)
    - [while, do while](43-while-do-while)
    - [for](#44-for)
    - [foreach](#45-foreach)
    - [try, catch](#46-try-catch)
- [5. 클로저](#5-closures)
- [6. 예시](#6-위의-사항들을-반영한-예시)
    

## 1. 일반적인 내용

### 1.1 PHP 구문 관련

- php 태그는 `반드시` `<?php` 혹은 `<?=` 시작해야 합니다. (PHP 짧은 태그 허용 X)

~~~php
// 좋은 예
<?php

// 나쁜 예
<?
~~~

- PHP 한 file에서 `class`, `function`, `상수` 같은 선언 내용과 `side effects(출력, 변경, ini설정)` 등을 `되도록` 혼용시키지 않도록 해야합니다.
~~~php
<?php

// side effect: change ini settings
ini_set('error_reporting', E_ALL);

// side effect: loads a file
include "file.php";

// side effect: generates output
echo "<html>\n";

// declaration
function foo()
{
    // function body
}
~~~
아래와 같이 함수 정의구문에 side effect 가 발생 안되도록 처리해 주세요.
~~~php
<?php

// 좋은 예
if (! function_exists('bar')) {
    function bar()
    {
        // function body
    }
}

// 나쁜 예
function foo()
{
    // function body
}
~~~


### 1.2 FILE 관련

- 파일은 `BOM(Byte order Mark)`없는 `UTF-8`만 사용해야 합니다.
- 파일의 개행문자는 `반드시` LF(Line Feed, \n)를 사용합니다.
- PHP구문만 있는 파일에 PHP 닫는 태그 `?>`는 `반드시` `생략` 합니다.


### 1.3 Line 관련

- 빈줄이 아닐 경우 `반드시` 줄 맨끝에 공백문자가 있으면 안됩니다.
- 코드의 가독성을 위해 빈줄을 추가`하셔도` 됩니다.
- 한줄에 하나 이상의 문장이 있으면 안됩니다.
- 한줄의 글자 수 한도는 120로 `권장` 합니다. (경고는 표기되지만 오류는 X)
- 모든 PHP 파일은 `반드시` 맨 마지막에는 하나의 빈줄로 끝나야합니다.
~~~php
<?php

// 좋은 예
class foo()
{
    // class body
}
 
~~~
~~~php
<?php

// 나쁜 예
class foo()
{
    // class body
}
~~~


### 1.4 들여쓰기

- `반드시` 들여쓰기를 스페이스 4개로 사용해야합니다. (tab 안됨)

```
* 공백만 사용하면 git에서의 fetch / diff 문제를 피할 수 있으며,
정렬이 세분화되어 하위 들여쓰기를 쉽게 삽입이 가능합니다.
```


### 1.5 PHP Keyword 와 true/false/null 

- php [keyword](http://php.net/manual/en/reserved.keywords.php) 와 `true`, `false`, and `null` 등은 `반드시` `소문자`로 입력해야 합니다.
~~~php
<?php

// 좋은 예
if ($flag === true) {
    
}

// 나쁜 예
if ($flag === True) {
    
}
// 나쁜 예
if ($flag === TRUE) {
    
}
~~~

## 1.6 비교연산자

비교연산자 사용시 `==` 보다 `===`를 사용하세요. PHP의 `==` 연산자는 신뢰가 불가능합니다.
예를들어 아래와 같은 구문은 전부 `true`입니다.

~~~php
<?php

// true
var_dump(1 == true);
var_dump(['1'] == true);
var_dump('1' == true);
// true
var_dump(0 == "a");
var_dump("1" == "01");
var_dump("1" == "1e0");
// true
var_dump('' == false);
var_dump([] == false);
var_dump(null == false);
~~~

이러한 특성이 있다보니 데이터가 비어있는지 혹은 `''` 같은 빈문자열 검사를 `==` 혹은 `if ($flag)` 같은 방식으로 검사하게되면
논리 오류가 발생할 가능성이 높습니다. 이러한 언어에서는 `===`를 사용하는게 논리 오류를 발생시킬 여지를 줄일 수 있습니다.

## 1.7 삼항연산자

PHP는 단일 문장 내에서 둘 이상의 삼항연산자를 사용하면 제대로 동작하지 않습니다.
`반드시` 단일 삼항연산자로만 사용해 주세요.

~~~php
<?php

// 언뜻보기에 다음과 같이 'true'가 출력됩니다
echo (true?'true':false?'t':'f');

// 그러나 위의 실제 출력은 't'입니다.
// 이것은 삼항 표현식이 왼쪽에서 오른쪽으로 계산되기 때문입니다.

// 다음은 위와 같은 코드의 더 정확한 코드입니다.
echo ((true ? 'true' : false) ? 't' : 'f');

// 여기서 첫 번째 표현식이 'true'로 평가된다는 것을 알 수 있습니다.
// 그러면 true를 평가하여 (bool) true를 반환합니다.
~~~



## 2 Namespace 와 Use 선언 관련 

- 네임스페이스와 클래스는 오토로딩 표준( ~~PSR-0~~, [PSR-4](http://www.php-fig.org/psr/psr-4/) )를 준수해야 합니다.
~~~php
<?php

namespace \<NamespaceName>(\<SubNamespaceNames>)*\<ClassName>

// 좋은 예
namespace App\Models\User;

// 나쁜 예
namespace App\models\User;
~~~

- 네임스페이스 선언문과 use 선언구문 다음 한줄을 `반드시` 비워두시기 바랍니다.
~~~php
<?php

// 좋은 예
namespace App\Providers;

use Illuminate\Support\Facades\App;
use Illuminate\Support\Facades\Config;

class AppServiceProvider extends ServiceProvider
{

// 나쁜 예
namespace App\Providers;
use Illuminate\Support\Facades\App;
use Illuminate\Support\Facades\Config;
class AppServiceProvider extends ServiceProvider
{
~~~

- use문은 `반드시` 한줄씩 선언해야 합니다.
~~~php
<?php

// 좋은 예
use Illuminate\Support\Facades\App;
use Illuminate\Support\Facades\Config;

// 나쁜 예
use Illuminate\Support\Facades\{App, Config};

// 나쁜 예
use Illuminate\Support\Facades\{
    App,
    Config
};
~~~

## 3. 클래스, 속성, 메서드

여기서 말하는 클래스 `class`, `interface`, `trait`를 뜻합니다.

### 3.1 class 정의구문

- 클래스 이름은 `반드시` 다음과 같은 `StudlyCaps` 로 작성해야 합니다.
~~~php
<?php

// 좋은 예
class StudlyCaps
{

// 나쁜 예
class camelCase
{
~~~

- 클래스와 클래스 메서드의 여는 중괄호 `{`는 선언문 다음 줄에 있어야 하며, 닫는 중괄호 `}`는 본문 다음 중에 위치해야 합니다.
~~~php
<?php

// 좋은 예
class StudlyCaps
{
    public function camelCase()
    {
        //...
    }
}  

// 나쁜 예
class StudlyCaps {
    public function camelCase() {
        //...
    }
}
~~~

### 3.2 Extends 와 Implements

- `extends` and `implements` 키워드는 `반드시` class 명과 같은 줄에 위치해야 합니다. 
~~~php
<?php
namespace Vendor\Package;

use FooClass;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class ClassName extends ParentClass implements \ArrayAccess, \Countable
{
    // constants, properties, methods
}
~~~

### 3.3 클래스 상수(const)

- 클래스 `상수(const)` 명칭은 `반드시` `대문자`와 `_`(underscore)를 이용하여 정의해 주시기 바랍니다.
~~~php
<?php

// 좋은 예
class StudlyCaps
{
    public const UPPER_CASE = 'text';
    
// 나쁜 예
class StudlyCaps
{
    public const camelCase = 'text';
    public const snake_case = 'text';
    public const StudlyCaps = 'text';
~~~

### 3.4 클래스 Properties(멤버 변수)

- `var` keyword를 이용하여 멤버변수를 선언해서는 안됩니다.
~~~php
<?php

class ClassName
{
    // 좋은 예
    public $foo = "";
    
    // 나쁜 예
    var $foo = '';
}
~~~
- 한줄에 여러개 `property(멤버변수)`를 선언해서는 안됩니다.
- `property(멤버변수)` 이름에 맨 앞에 `_(underscore)`로 `private` 암시적으로 표현해서는 안됩니다.

- 클래스 `속성(properties)`은 `$StudlyCaps`, `$camelCase`, 혹은 `$under_score`으로 사용 가능하지만, 일관성을 위해 `$camelCase`를 사용합니다.
~~~php
<?php

class StudlyCaps
{
    // 좋은 예
    public $camelCase = '';
    
    // 나쁜 예
    public $StudlyCaps = '';
    public $under_score = '';
~~~

`property(멤버변수)`에 대한 예시는 아래와 같습니다.

~~~php
<?php
namespace Vendor\Package;

class ClassName
{
    public $fooBar = null;
}
~~~


### 3.5 클래스 메서드

- 클래스 `메서드` 명칭은 `반드시` `camelCase`와 같이 정의해 주시기 바랍니다.
~~~php
<?php

// 좋은 예
class StudlyCaps
{
    public function camelCase()
    {

// 나쁜 예
class StudlyCaps
{
    public function snake_case()
    {
~~~

- 클래스의 `메서드`와 `속성(properties)`에는 `반드시` `Visibility(private, public, protected)`가 선언되어 있어야 하며, 
`abstract`, `final`은 `Visibility` 앞에 선언되어 있어야 하고 `static`은  `Visibility` 뒤에 선언되어야 합니다.
~~~php
<?php

class StudlyCaps
{
    // 좋은 예
    public static function camelCase()
    {
    }
    
    // 좋은 예
    final public function camelCase()
    {
    }
    
    // 나쁜 예
    static public function camelCase()
    {
    }
~~~

- 메서드 이름 맨앞에 `_(underscore)`로 `private` or `protected` 암시적으로 표현해서는 안됩니다.
- 메서드 이름과 여는괄호 `(` 사이 공백이 있어서는 안됩니다.
- 메서드 `매개변수(parameter)` 각 쉼표 앞에 공백이 있으면 안됩니다.
- 메서드 `매개변수(parameter)` 중 디폴트 값을 가진 매개변수는 `반드시` 맨 끝에 위치해야 합니다. 

클래스 메서드는 최종적으로 아래의 예시처럼 작성해 주시기 바랍니다.
~~~php
<?php

namespace Vendor\Package;

class ClassName
{
    public function fooBarBaz($arg1, &$arg2, $arg3 = [])
    {
        // method body
    }
}
~~~

### 3.6 함수 혹은 메서드 호출

함수 혹은 메서드 호출은 아래와 같이 작성하시면 됩니다. (괄호, 중괄호, 공백를 중점으로 확인하세요.)
~~~php
<?php
bar();
$foo->bar($arg1);
Foo::bar($arg2, $arg3);
~~~


## 4. 제어문

제어문을 사용시 일반적으로 지켜야 할 규칙은 아래와 같습니다.

- 제어문 keyword(`if`, `else`, `for` 등) 다음에는 공백 한칸이 있어야 합니다.
- 제어문 여는 괄호 `(` 뒤에 공백이 있으면 안됩니다.
- 제어문 닫는 괄호 `)` 앞에 공백이 있으면 없어야합니다.
- 제어문 닫는 괄호와 `)` 여는 중괄호 `{` 사이에 공백 한칸이 있어야 합니다.
- 제어문 중괄호는 `반드시` 입력해야 합니다. (단일 제어문의 경우 실수가 발생 될 가능성이 높습니다.)
- 제어문 괄호 안에 메서드나 함수 호출은 하지마세요. (제어문 돌때마다 함수구문이 실행됩니다.)
~~~php
<?php

// 좋은 예
if ($flag === true) {
    
}

// 나쁜 예
// 제어문 keyword 다음에 공백이 없음
if($flag === true) {
    
}

// 나쁜 예
// 제어문 여는 괄호 뒤 공백이 존재
// 제어문 닫는 괄호 앞에 공백이 존재 
if( $flag === true ) {
    
}

// 나쁜 예
// 제어문 여는 중괄호 앞에 공백이 없음
if ($flag === true){
    
}

// 나쁜 예
// 단일 제어문
if ($flag === true) $content = "";

// 나쁜 예
// 단일 제어문
if ($flag === true)
    $content = '';

// 나쁜 예
// 제어문 괄호 안에서 메서드나 함수 호출 사용
for ($i = 0; $i < $users->count(); $i++) {
    
}
~~~

### 4.1 `if`, `elseif`, `else`

`if`, `else`, `elseif` 구문에는 중괄호가 `반드시` 있어야하며 `else if`는 사용하면 안됩니다.
~~~php
<?php

if ($expr1) {
    // if body
} elseif ($expr2) {
    // elseif body
} else {
    // else body;
}
~~~


### 4.2 `switch`, `case`

`switch` 구문은 아래와 같이 사용합니다. (괄호, 중괄호, 공백를 중점으로 확인하세요.)
`break` 문은 `case` 구문 본문과 동일한 들여쓰기로 작성해 주시기 바랍니다. 

~~~php
<?php
switch ($expr) {
    case 0:
        echo 'First case, with a break';
        break;
    case 1:
        echo 'Second case, which falls through';
        break;
    case 2:
    case 3:
    case 4:
        echo 'Third case, return instead of break';
        break;
    default:
        echo 'Default case';
        break;
}
~~~

### 4.3 `while`, `do while`

`while`문은 아래와 같이 사용합니다. (괄호, 중괄호, 공백를 중점으로 확인하세요.)

~~~php
<?php

while ($expr) {
    // structure body
}
~~~

`do while`문은 비슷합니다. 아래와 같이 사용합니다. (괄호, 중괄호, 공백를 중점으로 확인하세요.)

~~~php
<?php

do {
    // structure body;
} while ($expr);
~~~


### 4.4 `for`

`for`문은 아래와 같이 사용합니다. (괄호, 중괄호, 공백를 중점으로 확인하세요.)

~~~php
<?php
for ($i = 0; $i < 10; $i++) {
    // for body
}
~~~

- for 문 괄호 안에서 함수나 메서드를 사용하지 마세요.
~~~php
<?php

// 나쁜 예
for ($i = 0; $i < $users->count(); $i++) {
    
}

// 좋은 예
$userCount = $users->count();

for ($i = 0; $i < $userCount; $i++) {
    
}
~~~

### 4.5 `foreach`

`foreach` 문은 아래와 같이 사용합니다. (괄호, 중괄호, 공백를 중점으로 확인하세요.)

~~~php
<?php
foreach ($iterable as $key => $value) {
    // foreach body
}
~~~


### 4.6. `try`, `catch`

`try catch` 구문은 아래와 같이 사용합니다. (괄호, 중괄호, 공백를 중점으로 확인하세요.)

~~~php
<?php
try {
    // try body
} catch (FirstExceptionType $e) {
    // catch body
} catch (OtherExceptionType $e) {
    // catch body
}
~~~

PHP에서는 Error와 Exception이 구분이 되다보니 Exception만으로는 Error 캐치가 불가능합니다.

PHP5 에서는 Error catch가 불가능하기 때문에 이러한 부분을 해결하기 위해 PHP 7 에서 부터는 Throwable 인터페이스를 사용하여 해결 할 수 있습니다.
  
~~~php
<?php

// PHP7 에서 Error 캐치
try {

} catch (Exception $e) {

} catch (Error $e) {

}

// 아래와 같이 요약 가능
try {

} catch (Throwable $t) {

}
~~~

## 5. Closures

`클로저`는 `반드시` `function` 키워드 다음에 공백으로 선언해야 하며, `use` 키워드 앞뒤로 공백 한칸을 넣어주세요.
~~~php
<?php
$closureWithArgs = function ($arg1, $arg2) {
    // body
};

$closureWithArgsAndVars = function ($arg1, $arg2) use ($var1, $var2) {
    // body
};
~~~


## 6. 위의 사항들을 반영한 예시

~~~php
<?php

namespace Vendor\Package;

use FooInterface;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class Foo extends Bar implements FooInterface
{
    public function sampleMethod($a, $b = null)
    {
        if ($a === $b) {
            bar();
        } elseif ($a > $b) {
            $foo->bar($arg1);
        } else {
            BazClass::bar($arg2, $arg3);
        }
    }

    final public static function bar()
    {
        // method body
    }
}
~~~
