基本コーディング規約
=====================

この規約セクションでは、共有されるPHPコードにおいて高い技術レベルでの連携を確保するために必要とされる標準的なコーディング要素を考慮したうえで構成されています。

文書内記載されている "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY" 及び "OPTIONAL" は、[RFC 2119][]で説明される趣旨で解釈してください。

[RFC 2119]: http://www.ietf.org/rfc/rfc2119.txt
[PSR-0]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-0.md


1. 概要
-----------

- PHPコードは「<?php」及び 「<?=」タグを使用しなければなりません。

- 文字コードはUTF-8（BOM無し）を使用しなければなりません。

- ファイルに定義するクラス、関数、定数などに副作用があってはいけません。仮にあった場合でも定義と処理を同時に実行すべきではありません。

- 名前空間、クラスについては[PSR-0][]に準拠しなければなりません。

- クラス名は、StudlyCaps（１文字目を除き、単語の先頭文字を大文字で表記する記法）記法で定義しなければなりません。

- クラス定数は全て大文字とし、区切り文字にはアンダースコアを用いて定義しなければなりません。

- メソッド名はcamelCase記法で定義しなければなりません。


2. ファイル
--------

### 2.1. PHPタグ

PHPコードは「<?php ?>」または短縮記述の「<?= ?>」を使用しなければなりません。それ以外のタグを使用してはいけません。

### 2.2. 文字コード

文字コードは、UTF-8（BOM無し）でなければなりません。

### 2.3. 副作用

ファイルに定義するクラス、関数、定数などに副作用があってはいけません。
仮にあった場合でも定義と処理を同時に実行すべきではありません。

ここでの「副作用」とは、クラス、関数、定数などに対して意図しないロジックが含まれ、実行されてしまうことを意味します。

なお、副作用範囲に含まれないものとして、ジェネレーターによる出力、明示的な「require」や「include」の使用、外部サービスへの接続、iniファイルの設定修正、エラーや例外の発行、グローバル変数や静的変数の修正、ファイルからの読み込み・書き込みなどが挙げられます。

以下は、定義と副作用両方が実装された例です。

```php
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
```

以下は、副作用が存在しない実装例です。


```php
<?php
// declaration
function foo()
{
    // function body
}

// conditional declaration is *not* a side effect
if (! function_exists('bar')) {
    function bar()
    {
        // function body
    }
}
```


3. 名前空間とクラス名
----------------------------

[PSR-0][]に準拠しなければなりません。

各クラスが、（トップレベルのベンダー名のように）少なくとも１レベルの名前空間となります。

クラス名は、StudlyCaps記法で定義しなければなりません。

PHP 5.3以降では、正しい名前空間を使用しなければなりません。

例:

```php
<?php
// PHP 5.3 and later:
namespace Vendor\Model;

class Foo
{
}
```

PHP 5.2以前では、クラス名に「Vendor_」接頭辞を使用し、擬似名前空間とする必要があります。

```php
<?php
// PHP 5.2.x and earlier:
class Vendor_Model_Foo
{
}
```

4. クラス定数、プロパティ及びメソッドについて
-------------------------------------------

ここでのクラスは、全ての一般クラス、インターフェイス、traitを含みます。

### 4.1. 定数

クラス定数は総じてアンダースコア文字を区切り文字として大文字で定義しなければなりません。
例:

```php
<?php
namespace Vendor\Model;

class Foo
{
    const VERSION = '1.0';
    const DATE_APPROVED = '2012-06-01';
}
```

### 4.2. プロパティ

このガイドでのプロパティ勧告（$StudlyCaps、$camelCase、$under_score）は意図的に回避・変更することができます。

どのような命名規則が使用されるかは、適切なスコープ内にて統一されている必要があります。
ここでのスコープは、ベンダーレベル、パッケージレベル、クラスレベルまたはメソッドレベルを指します。

### 4.3. Methods

メソッド名はcamelCase記法で定義しなければなりません。
