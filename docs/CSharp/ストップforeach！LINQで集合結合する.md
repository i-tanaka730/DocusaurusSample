## 1. はじめに
[2年間C#プロジェクトに携わった中でよく使ってきたLINQまとめ](https://qiita.com/i-tanaka730/items/5bbe3ed55092a9a21c23)
に引き続きLINQ記事です。
LINQを知らないと、コレクションを扱う際に、ついforeachを使ってしまうかと思います。
そのforeach、ちょっと待ってください！！！
LINQを使って簡単に集合・結合操作が行えるのでまとめていきたいと思います。


## 2. 集合

### 2-1. Union (和集合)
コレクション同士を連結します。
重複するデータがある場合は1件のみに絞られます。

```C#
var listA = new string[] { "りんご", "ばなな", "ぶどう", "みかん" };
var listB = new string[] { "さくらんぼ", "ばなな", "もも", "りんご" };

var list = listA.Union(listB);
	
foreach (var item in list)
{
	System.Console.WriteLine(item);
}
```
**＜結果＞**
![Union.PNG](https://qiita-image-store.s3.amazonaws.com/0/247638/a9a14112-eb8b-f88e-9f42-16b588b52043.png)

### 2-2. Concat (和集合)
コレクション同士を連結します。
重複するデータがある場合はそのまま連結されます。(Union Allのイメージです。)

```C#
void Main()
{
	var listA = new string[] { "りんご", "ばなな", "ぶどう", "みかん" };
	var listB = new string[] { "さくらんぼ", "ばなな", "もも", "りんご" };

	var list = listA.Concat(listB);
	
	foreach (var item in list)
	{
		System.Console.WriteLine(item);
	}
}
```
**＜結果＞**
![Concat.PNG](https://qiita-image-store.s3.amazonaws.com/0/247638/ff9f8c68-af10-df62-aa2d-5b21a7aab0db.png)



### 2-3. Intersect (積集合)
2つのコレクションのどちらにも含まれている要素のみ抽出します。

```C#
var listA = new string[] { "りんご", "ばなな", "ぶどう", "みかん" };
var listB = new string[] { "さくらんぼ", "ばなな", "もも", "りんご" };

var list = listA.Intersect(listB);
	
foreach (var item in list)
{
	System.Console.WriteLine(item);
}
```
**＜結果＞**
![Intersect.PNG](https://qiita-image-store.s3.amazonaws.com/0/247638/67c29164-7a9c-39f8-2863-2f5b2cec18f9.png)

### 2-4. Except (差集合)
左側のコレクション要素から、右側のコレクション要素を引きます。

```C#
var listA = new string[] { "りんご", "ばなな", "ぶどう", "みかん" };
var listB = new string[] { "さくらんぼ", "ばなな", "もも", "りんご" };

var list = listA.Except(listB);
	
foreach (var item in list)
{
	System.Console.WriteLine(item);
}
```
**＜結果＞**
![Except.PNG](https://qiita-image-store.s3.amazonaws.com/0/247638/f13609f7-2524-d0d5-03d7-e5c1b67b2a79.png)

## 3. 結合

### 3-1. Join (内部結合)
指定したキーに基づいて、コレクションを内部結合します。
以下の例では「社員.部署ID」と「部署.部署ID」をキーとして結合し、
1つのコレクションを作成しています。

```C#
// 従業員
public class Employee
{
	// 従業員ID
	public int Id { get; set;}
	// 部署ID
	public int DepartmentId { get; set;}
	// 従業員名
	public string Name { get; set;}
}

// 部署
public class Department
{
	// 部署ID
	public int Id { get; set;}
	// 部署名
	public string Name { get; set;}
}

// 従業員データ
var employeeList = new List<Employee>
{
	new Employee { Id = 100, Name = "佐藤", DepartmentId = 1 },
	new Employee { Id = 101, Name = "鈴木", DepartmentId = 2 },
	new Employee { Id = 102, Name = "高橋", DepartmentId = 3 },
	new Employee { Id = 103, Name = "田中", DepartmentId = 2 },
	new Employee { Id = 104, Name = "伊藤", DepartmentId = 1 },
	new Employee { Id = 105, Name = "渡辺", DepartmentId = 3 },
	new Employee { Id = 106, Name = "山本", DepartmentId = 1 },
};

// 部署データ
var departmentList = new List<Department>
{
	new Department { Id = 1, Name = "開発部" },
	new Department { Id = 2, Name = "研究部" },
	new Department { Id = 3, Name = "総務部" }
};

var list = employeeList.Join(departmentList, e => e.DepartmentId, d => d.Id, (e, d) => new
{
	EmployeeId = e.Id,
	EmployeeName = e.Name,
	DepartmentName = d.Name
});

foreach (var item in list)
{
	System.Console.WriteLine(item);
}

```
**＜結果＞**
![Join.PNG](https://qiita-image-store.s3.amazonaws.com/0/247638/3a5c41c5-01fd-839b-fae1-a7e1ac359fe5.png)

### 3-2. GroupJoin (外部結合)
指定したキーに基づいて、コレクションを外部結合します。
以下の例では、「社員.部署ID」と「部署.部署ID」をキーとして結合しますが、
紐づくキーがない場合は部署にnullを設定するようにしています。

```C#
// 従業員
public class Employee
{
	// 従業員ID
	public int Id { get; set;}
	// 部署ID(null許容型)
	public int? DepartmentId { get; set;}
	// 従業員名
	public string Name { get; set;}
}

// 部署
public class Department
{
	// 部署ID
	public int Id { get; set;}
	// 部署名
	public string Name { get; set;}
}

void Main()
{
	// 従業員データ
	var employeeList = new List<Employee>
	{
		new Employee { Id = 100, Name = "佐藤", DepartmentId = 1 },
		new Employee { Id = 101, Name = "鈴木", DepartmentId = 2 },
		new Employee { Id = 102, Name = "高橋", DepartmentId = null },
		new Employee { Id = 103, Name = "田中", DepartmentId = null },
		new Employee { Id = 104, Name = "伊藤", DepartmentId = 1 },
		new Employee { Id = 105, Name = "渡辺", DepartmentId = 3 },
		new Employee { Id = 106, Name = "山本", DepartmentId = 1 },
	};

	// 部署データ
	var departmentList = new List<Department>
	{
		new Department { Id = 1, Name = "開発部" },
		new Department { Id = 2, Name = "研究部" },
		new Department { Id = 3, Name = "総務部" }
	};
	
	var list = employeeList.GroupJoin(departmentList, e => e.DepartmentId, d => d.Id, (e, d) => new
	{
		EmployeeId = e.Id,
		EmployeeName = e.Name,
		Departments = d.DefaultIfEmpty()
	})
	.SelectMany(d => d.Departments, (e, d) => new
	{
        EmployeeId = e.EmployeeId,
        EmployeeName = e.EmployeeName,
        DepartmentName = d != null ? d.Name : "所属なし"
    });

	foreach (var item in list)
	{
		System.Console.WriteLine(item);
	}
}
```
**＜結果＞**
![GroupJoin.PNG](https://qiita-image-store.s3.amazonaws.com/0/247638/3b896627-0588-9785-a095-d6e9aa74469b.png)


## 4. おわりに
コレクション操作はいろんな場面で使えるので、覚えておくととても便利ですね！
他にもありましたら随時更新させて頂きます。
