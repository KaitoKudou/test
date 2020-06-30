# 課題5 phpによる動的Webページ[発展課題]
## 課題内容
フォームからユーザ名とパスワードを入力するとユーザ名を表示するphpプログラムを作成する。
- phpがインストールされていないなら、インストールし、適切な設定をしなさい。
- 入力フォーム及び、表示用のphpプログラムのURLは以下のものとする。
  - http://sysadmin??.sysadfun.com/login.html
  - 演習室のネットワークの外から、Webブラウザでアクセスするには、XX班の場合であれば、Webブラウザで http://10.14.51.23:80XX/login.html
  - http://sysadmin??.sysadfun.com/login.php phpプログラム
  - 演習室のネットワークの外から、Webブラウザでアクセスするには、XX班の場合であれば、Webブラウザで http://10.14.51.23:80XX/login.php
  - 必要なら以下のサンプルファイルを使っても良い
    - https://s3-ap-northeast-1.amazonaws.com/niimilabwiki/sysadmin2020/login.html
    - https://s3-ap-northeast-1.amazonaws.com/niimilabwiki/sysadmin2020/login.php
    
## 手順
### 1. phpのインストール
1. rootにログイン
```
su
```
2. phpがインストールされているかどうか確認する。
```
yum list installed | grep php
```
3. インストールされていなかったら、phpをインストールする。
```
yum install php
```
### 2. Apacheの再起動
```
systemctl restart httpd
```
#### 参考
httpdが正常に動作しているかどうかは以下のコマンドで確かめることができる。
```
systemctl status httpd.service
```
正常に動いていれば、active(running)と表示される。
動作していなければ、inactive (dead)と表示される。
### 2. コードをPHPやHTMLで書く
1. viエディタを起動
```
vi /var/www/html/login.php
```
2. 以下をlogin.phpにコピペ
```
<html xmlns="http://www.w3.org/1999/xhtml" lang="ja">

    <head>
        <title>SysAdmin Web Question No.5 -Form-</title>
        <meta name="keywords" content="System Administration">
        <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
    </head>
    <body>
        <?php
            if(isset($_POST['user-name']) && isset($_POST['user-password']) &&
            	!empty($_POST['user-name']) && !empty($_POST['user-password'])) {
        ?>
        <h1 class="login-message">ログインありがとうございます</h1>
            <table border="0">
                <tr>
                    <th>ユーザ名：</th>
                    <th><?php echo htmlspecialchars($_POST['user-name']); ?></th>
                </tr>
                <tr>
                    <td>パスワード：</td>
                    <th>※セキュリティのため表示しません</th>
                </tr>
            </table>
        <?php
        } else {
            echo "<h1 class=\"error-message\">ERROR!: ログイン画面でユーザ名とパスワードを送信してください</h1>";
        }
        ?>
        <hr />
        <p><a href="./login.html">ログイン画面に戻る</a></p>
    </body>
</html>

```
4. viエディタを起動(4, 5は順不同)
```
vi /var/www/html/login.html
```
5. 以下をlogin.phpにコピペ(4, 5は順不同)
```
<html xmlns="http://www.w3.org/1999/xhtml" lang="ja">

    <head>
        <title>SysAdmin Web Question No.5 -Form-</title>
        <meta name="keywords" content="System Administration">
        <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
    </head>
    <body>
        <h1>SysAdmin Web Question No.5 -Form-</h1>
        <p class="description">POSTメソッドで送ればURLに何も見えないから安全だね！！！</p>
        <hr />

        <form action="./login.php" method="post">
            <table border="0">
                <tr>
		            <th>ユーザ名：</th>
		            <th><input type="text" name="user-name" size="40"></th>
                </tr>
                <tr>
		            <td>パスワード：</td>
		            <td><input type="password" name="user-password" size="40"></td>		
                </tr>
            </table>

            <p class="form-button">
                <input type="submit" value="送信">
                <input type="reset" value="リセット">
            </p>
        </form>	
        <?php if ($logged_in): ?>
            <?php echo "こんにちは".$_POST["username"]."さん"?>
        <?php elseif (!empty($_POST["username"])):?>
            <?php echo $_POST["username"]."さんが見つかりませんでした。入力情報を確認してください"?>
        <?php endif;?>
    </body>
</html>
```
6. WEBブラウザで確認
```
http://10.14.51.23:80XX/login.html
```
XXには自身の班の番号が入る。

## キーワード
### word
- php
- Apache
### directory
- /var/www/html/
### command
- httpd
- yum
- su
- vi
- systemctl
## リファレンス
- [PHPとLinuxとApacheとMySQLを組み合わせてみよう](https://hacknote.jp/archives/55908/)
- [Linux(CentOS7)でWebサーバーを構築する。 -php導入編-](https://qiita.com/sango/items/a08c5b04df7125aaaad3)
- [PHPの設定内容を確認 - phpinfo()](https://webkaru.net/php/function-phpinfo/)