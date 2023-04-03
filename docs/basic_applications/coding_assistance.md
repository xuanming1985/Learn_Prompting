---
sidebar_position: 40
---

# 🟢 编程辅助

您可以使用ChatGPT进行调试、代码生成、格式化、注释等等操作，它可以处理40多种编程语言的代码。

|   极古老  |         古老    |     近期          |    先进         |
|-------------|----------------|---------------------|-----------------------|
| BASIC       | Perl           | Swift               | Kotlin                |
| Assembly    | Pascal         | TypeScript          | Julia                 |
| Fortran     | PHP            | Rust                | Crystal               |
| Lisp        | Prolog         | Kotlin/Native       | Racket                |
| COBOL       | C              | Julia (GPU)         | Lua                   |
| Algol       | PL/I           | Go                  | Zig                   |
| SNOBOL      | C++            | Dart                | Nim                   |
| RPG         | Smalltalk      | Elixir              | Crystal (LLVM)        |
| Forth       | Tcl            | Groovy              | Vlang                 |
| Ada         | SQL            | Scala Native        | Erlang                |

## 代码生成

让ChatGPT生成代码非常简单，只需要这样：

```text
请假设你是一名初级的Python程序员。请写一段代码，生成三个由6个项目组成的数组的madlibs，其中prepositions代表介词, verbs代表动词, nouns代表名词，并且名词也被用作直接对象。编写完后，请执行该代码。
```

输出结果将类似于：

```text
// highlight-start
import random

prepositions = ['at', 'in', 'on', 'with', 'under', 'over']
verbs = ['run', 'jump', 'dance', 'sing', 'laugh', 'cry']
nouns = ['cat', 'dog', 'tree', 'flower', 'book', 'computer']

preposition = random.choice(prepositions)
verb = random.choice(verbs)
noun = random.choice(nouns)

madlib = f"I {verb} {preposition} the {noun}."

print(madlib)
// highlight-end
```

在此处尝试：

<div 
trydyno-embed="" 
openai-model="text-davinci-003" 
initial-prompt="请假设你是一名初级的Python程序员。请写一段代码，生成三个由6个项目组成的数组的madlibs，其中prepositions代表介词, verbs代表动词, nouns代表名词，并且名词也被用作直接对象。" 
initial-response="" 
max-tokens="512" 
box-rows="5" 
model-temp="0.0" 
top-p="0">
    <noscript>无法加载Dyno Embed:必须启用JavaScript</noscript>
</div>

## 代码注释和格式化

您可以请求ChatGPT为您的代码添加注释并格式化。在输入样本之后，添加三个井号然后粘贴要清理的代码：

```text
请为此Python代码添加行注释并将其格式化以便阅读：
  
import math
amplitude = 30
frequency = 2
num_points = 50
for i in range(num_points):
    y = int(round(amplitude * math.sin(2 * math.pi * frequency * i / num_points)))
    print(' ' * (amplitude + y) + '*')
```

输出结果将类似于：

```text
// highlight-start
# 导入math库
import math

# 设置波浪的振幅，频率和点数
amplitude = 30
frequency = 2
num_points = 50

# 遍历波浪上的每个点
for i in range(num_points):
    # 使用math库中的正弦函数计算波浪的y值
    y = int(round(amplitude * math.sin(2 * math.pi * frequency * i / num_points)))
    
    # 打印一个填充空格，后跟位于波浪上适当位置的星号
    print(' ' * (amplitude + y) + '*')

// highlight-end
```

## 调试

ChatGPT不仅可以检测代码中的语法错误，还可以查找在执行代码时会出现的逻辑错误。以下是一个Python脚本的示例，最终由于第3行的逻辑错误导致第4行出现除以零的错误。 尝试这个简单的提示，找到并修复错误：

```text
请调试此Python代码：  

1. pizza_cost = 100
2. partygoers = 10
3. while partygoers = 0:
4.    cost_per_person = pizza_cost / partygoers
5.    print("the cost per person is $", "{:.2f}".format(cost_per_person))
6.    partygoers -= 2
```

输出将包括更正的while循环，该循环已更改以确保分母变量保持为正数：

```text
// highlight-start
3. while partygoers > 0:
// highlight-end
```

对于更复杂的错误，您可能需要添加更多的细节：错误消息、环境变量、包含的库或输入变化。

## 代码优化

如果您的脚本执行时间过长，ChatGPT可以对其进行优化。以下是优化一个找质数的低效脚本的示例：

```text
请假设你是一名非常有经验的Python开发者。请优化此脚本：

for num in range(1, 101):
    if num  1:
        for i in range(2, num):
            if (num % i) == 0:
                break
        else:
            print(num)
```

输出可能是使用[Sieve of Eratosthenes算法](https://www.geeksforgeeks.org/sieve-of-eratosthenes/)查找质数的脚本：

```text
// highlight-start
numbers = list(range(2, 101))
for i in numbers:
    for j in range(i * 2, 101, i):
        if j in numbers:
            numbers.remove(j)
print(numbers)
// highlight-end
```

请注意，我们要求机器人“假装成 _非常有经验的_ 开发者”来优化脚本。您还可以规定它在某个特定领域具有专业知识（例如，排序算法）或具有某些年限的经验。或者，如果您有一个看起来过于复杂的脚本，可以要求ChatGPT以“ _非常初级的_ 开发者”的身份编写该脚本。
## 在编程语言之间进行转换

ChatGPT提供的一个非常有用的功能是能够将程序从一种语言移植到另一种语言。一个典型的用例可能是将旧语言中的代码升级到更现代的语言：

```text
表现得像了解COBOL和Python的经验丰富的开发人员。请将此COBOL程序转换为Python：  


IDENTIFICATION DIVISION.
PROGRAM-ID. CALCULATE-SS-TAX.

DATA DIVISION.
WORKING-STORAGE SECTION.
01 SALARY PIC 9(7)V99.
01 SS-TAX PIC 9(7)V99.

PROCEDURE DIVISION.
DISPLAY "Enter employee's salary: ".
ACCEPT SALARY.

COMPUTE SS-TAX = SALARY * 0.062.

DISPLAY "Social Security tax is: $", SS-TAX.
STOP RUN.
```

输出可能类似于：

```text
// highlight-start
salary = float(input("Enter employee's salary: "))
ss_tax = salary * 0.062

print("Social Security tax is: $", round(ss_tax, 2))
// highlight-end
```


## 处理多个文件和重构您的代码库

如果您想要ChatGPT评估**多个**文件怎么办？ 只需提供文件名并粘贴每个文件的代码即可：

```text
我的Python脚本使用两个文件，分别在不同的目录中。请显示执行conversation.py时的输出：

BEGIN FILE 1: ./phrases/coming_and_going.py
def hi(name):
   print(f"Hello, {name}!")
def bye(name):
   print(f"Goodbye, {name}!")

BEGIN FILE 2: ./conversation.py
from phrases.coming_and_going import hi, bye
hi("John")
bye("John")

// highlight-start
Hello, John!
Goodbye, John!
// highlight-end
```

使用此技术，您可以在单个提示中提交多个文件到ChatGPT并要求其重构代码。 或者，您可以要求它“记忆”每一个，然后在稍后的提示中执行它。

## 模拟数据库服务器

ChatGPT不仅可以处理代码，还可以模拟诸如数据库、Web和电子邮件服务器等服务器环境。考虑此提示：

```text
扮演Microsoft SQL Server。创建名为“politics”的数据库，其中包含名为“politicians”的表格。将50行来自不同年代（1900-2000年）世界各地著名政客的信息填入其中。为他们的全名、国家、出生日期和适用的死亡日期添加列。创建一个查看前3位活得最久的政治家的视图。创建并执行一个Transact-SQL命令，输出该视图的内容。
```

输出将类似于：

```text
// highlight-start
-- 创建名为“politics”的数据库
CREATE DATABASE politics;
GO

-- 使用名为“politics”的数据库
USE politics;
GO

-- 创建名为“politicians”的表格
CREATE TABLE politicians (
    id INT PRIMARY KEY,
    full_name VARCHAR(50),
    country VARCHAR(50),
    date_of_birth DATE,
    date_of_death DATE
);
-- 等等。
// highlight-end
```

从那里，您可以添加列、索引等：

```text
在politicians表格中添加一个副手列，并为每行更新它。
```

输出可能类似于：

```text
// highlight-start
ALTER TABLE politicians ADD second_in_command VARCHAR(255);

UPDATE politicians
SET second_in_command = 
    CASE name
        WHEN 'Winston Churchill' THEN 'Clement Attlee'
        WHEN 'Franklin D. Roosevelt' THEN 'Harry S. Truman'
        WHEN 'Mao Zedong' THEN 'Lin Biao'
        -- add more cases for each politician
        ELSE NULL
    END;
// highlight-end
```

## 模拟Web服务器

ChatGPT可以模拟许多其他服务器和API。这是一个模拟Apache服务器响应特定浏览器请求头的提示示例：

```text
扮演一个Apache Web服务器。您将如何回应这些HTTP标头？

GET /example HTTP/1.1
Host: www.example.com
```

输出将类似于：

```text
// highlight-start
HTTP/1.1 200 OK
Date: [当前日期和时间]
Server: Apache/2.4.41 (Ubuntu)
Last-Modified：[请求资源的上次修改日期和时间]
Content-Type：[所请求资源的MIME类型，例如text/html]
// highlight-end
```

以下是ChatGPT可以模拟的某些服务器和API类型：

|   很老      |     老        |        新近            |    前沿              |
|-------------|----------------|------------------------|----------------------|
| Telnet      | HTTP/1.0       | RESTful API            | GraphQL API          |
| FTP         | SMTP           | SOAP API               | GRPC API             |
| POP3        | LDAP           | OAuth 2.0              | Docker API           |
| IMAP        | DHCP           | OpenAPI（Swagger）     | Kubernetes API       |
| DNS         | NTP            | gRPC-web API           | Istio API            |
| SNMP        | RADIUS         | MQTT API               | Prometheus API       |
| Syslog      | SFTP           | WebSocket API          | Envoy API            |
| TFTP        | SSH            | AWS API Gateway        | Istio Mixer API      |
| NFS         | XML-RPC        | Azure API Management   | OpenTracing API      |


## 模拟命令行

由于Linux中的shell（或Windows PowerShell）也是一种编程语言，您可以使用ChatGPT与之交互，并为文件系统构建一个环境：

```
扮演Debian Linux命令shell。请像终端一样回应我的命令，尽可能简洁。我的第一个命令是：ls -l
```

输出将类似于：

```text
// highlight-start
-rw-r--r--  1 user  group   2048 Mar  3 14:23 file1.txt
drwxr-xr-x  2 user  group   4096 Feb 28 09:12 directory1
-rwx------  1 user  group  16384 Feb 25 19:41 executable1
// highlight-end
```

尝试尝试其他命令：

```text
>touch greeting.txt  
>echo "hello" > greeting.txt  
>ls -l  
```

ChatGPT应该会记住您的更改（至少是在一段时间内），最终输出应该类似于：

```text
// highlight-start
-rw-r--r--  1 user  group   2048 Mar  3 14:23 file1.txt
drwxr-xr-x  2 user  group   4096 Feb 28 09:12 directory1
-rwx------  1 user  group  16384 Feb 25 19:41 executable1
-rw-r--r--  1 user  group      6 Mar  4 16:15 greeting.txt
// highlight-end
```

有关使用ChatGPT作为虚拟机的完整讨论可在[engraved.blog](https://www.engraved.blog/building-a-virtual-machine-inside/)中找到。

---

由Prompt Yes! 提供[快速工程培训](https://promptyes.com/)。