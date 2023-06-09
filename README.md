# subject-230609

### MySQL 接続関数
```cs
static OdbcConnection CreateConnection()
{
    // 接続文字列の作成
    OdbcConnectionStringBuilder builder = new OdbcConnectionStringBuilder();
    builder.Driver = "MySQL ODBC 8.0 Unicode Driver";
    // 接続用のパラメータを追加
    builder.Add("server", "localhost");
    builder.Add("database", "lightbox");
    builder.Add("uid", "root");
    builder.Add("pwd", "");

    Console.WriteLine(builder.ConnectionString);
    Debug.WriteLine($"Debug:{builder.ConnectionString}");

    // 接続の作成
    OdbcConnection myCon = new OdbcConnection();

    // MySQL の接続準備完了
    myCon.ConnectionString = builder.ConnectionString;

    // MySQL に接続
    myCon.Open();

    return myCon;
}
```

### OdbcCommand と OdbcDataReader の使い方
```cs
// SQL実行用のオブジェクトを作成
OdbcCommand myCommand = new OdbcCommand();

// 実行用オブジェクトに必要な情報を与える
myCommand.CommandText = myQuery;    // SQL
myCommand.Connection = myCon;       // 接続

// 次でする、データベースの値をもらう為のオブジェクトの変数の定義
OdbcDataReader myReader;

// SELECT を実行した結果を取得
myReader = myCommand.ExecuteReader();

// myReader からデータが読みだされる間ずっとループ
while (myReader.Read())
{
    // 列名より列番号を取得
    int index = myReader.GetOrdinal("氏名");
    // 列番号で、値を取得して文字列化
    string text = myReader.GetValue(index).ToString();
    // 実際のコンソールに出力
    Console.WriteLine(text);
    // 出力ウインドウに出力
    Debug.WriteLine($"Debug:{text}");
}

myReader.Close();
```
### Form アプリケーションにおける GroupBox の状態変更
```cs
        private void 確認_Click(object sender, EventArgs e)
        {
            //Debug.WriteLine("確認");
            //MessageBox.Show("確認");

            this.ヘッド部.Enabled = false;
            this.ボディ部.Enabled = true;

        }

        private void キャンセル_Click(object sender, EventArgs e)
        {
            this.ヘッド部.Enabled = true;
            this.ボディ部.Enabled = false;

        }
```
![image](https://github.com/winofsql/subject-230609/assets/1501327/d7982c86-4dea-4a7f-8ef1-708372b3de25)

![image](https://github.com/winofsql/subject-230609/assets/1501327/a227a151-1832-4f73-a7de-cd9580a9297c)

### 🔴 入力フィールドを追加して、さらにリアルな画面遷移のコントロールを行う

![image](https://github.com/winofsql/subject-230609/assets/1501327/67278dd7-3137-42ba-a502-b4fe8d717fd0)

```cs
// 
// ヘッド部
// 
ヘッド部.Controls.Add(社員コード);
ヘッド部.Controls.Add(ラベル1);
ヘッド部.Controls.Add(ラベル2);
ヘッド部.Controls.Add(確認);
ヘッド部.Controls.Add(処理区分);
ヘッド部.Location = new Point(64, 56);
ヘッド部.Margin = new Padding(4);
ヘッド部.Name = "ヘッド部";
ヘッド部.Padding = new Padding(4);
ヘッド部.Size = new Size(559, 139);
ヘッド部.TabIndex = 2;
ヘッド部.TabStop = false;
ヘッド部.Text = "ヘッド部";
// 
// 社員コード
// 
社員コード.Location = new Point(140, 83);
社員コード.Name = "社員コード";
社員コード.Size = new Size(100, 23);
社員コード.TabIndex = 5;

// 
// ボディ部
// 
ボディ部.Controls.Add(給与);
ボディ部.Controls.Add(氏名);
ボディ部.Controls.Add(ラベル4);
ボディ部.Controls.Add(ラベル3);
ボディ部.Controls.Add(Label5);
ボディ部.Controls.Add(生年月日);
ボディ部.Controls.Add(更新);
ボディ部.Controls.Add(キャンセル);
ボディ部.Enabled = false;
ボディ部.Location = new Point(64, 218);
ボディ部.Margin = new Padding(4);
ボディ部.Name = "ボディ部";
ボディ部.Padding = new Padding(4);
ボディ部.Size = new Size(559, 241);
ボディ部.TabIndex = 3;
ボディ部.TabStop = false;
ボディ部.Text = "ボディ部";
// 
// 氏名
// 
氏名.Location = new Point(140, 49);
氏名.Name = "氏名";
氏名.Size = new Size(148, 23);
氏名.TabIndex = 6;

// 
// 給与
// 
給与.Location = new Point(140, 98);
給与.Name = "給与";
給与.Size = new Size(100, 23);
給与.TabIndex = 6;
```

### 🔴 画面遷移時のカーソルコントロールを行う

#### (1) 画面遷移時の初期カーソル

```cs
private void 確認_Click(object sender, EventArgs e)
{

    this.ヘッド部.Enabled = false;
    this.ボディ部.Enabled = true;

    this.氏名.Focus();
    this.氏名.SelectAll();

}

private void キャンセル_Click(object sender, EventArgs e)
{
    this.ヘッド部.Enabled = true;
    this.ボディ部.Enabled = false;

    this.社員コード.Focus();
    this.社員コード.SelectAll();

}
```

#### (2) カーソルの移動順序の調整( 表示メニューからタブオーダー )

![image](https://github.com/winofsql/subject-230609/assets/1501327/7e9a3296-4391-4c02-abe4-92251a968797)

※ 順番にクリックしていって決定する

### 入力した社員コードの存在チェック
```cs
private void 確認_Click(object sender, EventArgs e)
{
    // 接続文字列の作成
    OdbcConnectionStringBuilder builder = new OdbcConnectionStringBuilder();
    builder.Driver = "MySQL ODBC 8.0 Unicode Driver";
    // 接続用のパラメータを追加
    builder.Add("server", "localhost");
    builder.Add("database", "lightbox");
    builder.Add("uid", "root");
    builder.Add("pwd", "");

    Console.WriteLine(builder.ConnectionString);
    Debug.WriteLine($"Debug:{builder.ConnectionString}");

    // 接続の作成
    OdbcConnection myCon = new OdbcConnection();

    // MySQL の接続準備完了
    myCon.ConnectionString = builder.ConnectionString;

    // MySQL に接続
    myCon.Open();

    // SQL
    string myQuery =
        @$"SELECT * 
            from 社員マスタ where 社員コード = '{this.社員コード.Text}'";

    // SQL実行用のオブジェクトを作成
    OdbcCommand myCommand = new OdbcCommand();

    // 実行用オブジェクトに必要な情報を与える
    myCommand.CommandText = myQuery;    // SQL
    myCommand.Connection = myCon;       // 接続

    // 次でする、データベースの値をもらう為のオブジェクトの変数の定義
    OdbcDataReader myReader;

    // SELECT を実行した結果を取得
    myReader = myCommand.ExecuteReader();

    // myReader からデータが読みだされる間ずっとループ
    bool result = myReader.Read();
    if (result)
    {
        MessageBox.Show($"入力された社員コードは既に存在します : {this.社員コード.Text}");
        this.社員コード.Focus();
        this.社員コード.SelectAll();
        return;
    }


    myReader.Close();
    myCon.Close();


    this.ヘッド部.Enabled = false;
    this.ボディ部.Enabled = true;

    this.氏名.Focus();
    this.氏名.SelectAll();

}
```
![image](https://github.com/winofsql/subject-230609/assets/1501327/9638a14c-140b-4ce7-a5a9-22340765872a)

### 社員の新規更新
```cs
        private void 更新_Click(object sender, EventArgs e)
        {
            DialogResult result = MessageBox.Show(
                "更新してもよろしいですか?",
                "更新確認",
                MessageBoxButtons.YesNo,
                MessageBoxIcon.Question,
                MessageBoxDefaultButton.Button2
            );

            if (result == DialogResult.Yes)
            {
                // 何もしない
            }
            else
            {
                // 更新しないので処理を抜ける( No を選択 )
                this.氏名.Focus();
                this.氏名.SelectAll();
                return;
            }


            // 接続文字列の作成
            OdbcConnectionStringBuilder builder = new OdbcConnectionStringBuilder();
            builder.Driver = "MySQL ODBC 8.0 Unicode Driver";
            // 接続用のパラメータを追加
            builder.Add("server", "localhost");
            builder.Add("database", "lightbox");
            builder.Add("uid", "root");
            builder.Add("pwd", "");

            Console.WriteLine(builder.ConnectionString);
            Debug.WriteLine($"Debug:{builder.ConnectionString}");

            // 接続の作成
            OdbcConnection myCon = new OdbcConnection();

            // MySQL の接続準備完了
            myCon.ConnectionString = builder.ConnectionString;

            // MySQL に接続
            myCon.Open();

            // SQL
            string myQuery =
                @$"insert into `社員マスタ` (
	`社員コード` 
	,`氏名` 
	,`給与` 
	,`生年月日` 
)
 values(
	'{this.社員コード.Text}'
	,'{this.氏名.Text}'
	,{this.給与.Text}
	,'{this.生年月日.Value}'
)
";

            // SQL実行用のオブジェクトを作成
            OdbcCommand myCommand = new OdbcCommand();

            // 実行用オブジェクトに必要な情報を与える
            myCommand.CommandText = myQuery;    // SQL
            myCommand.Connection = myCon;       // 接続

            myCommand.ExecuteNonQuery();

            myCon.Close();

            this.社員コード.Clear();
            this.氏名.Clear();
            this.給与.Clear();
            this.生年月日.Value = DateTime.Now;

            this.ヘッド部.Enabled = true;
            this.ボディ部.Enabled = false;

            this.社員コード.Focus();
            this.社員コード.SelectAll();


        }
```

