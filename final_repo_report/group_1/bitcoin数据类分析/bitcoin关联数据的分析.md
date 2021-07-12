首先查询bitcoin项目活跃参与者信息，按降序排名，查询语句如下：

```sql
SELECT anyHeavy(actor_login) AS actor_login,
  SUM( issue_comment) AS ic, 
 SUM( open_issue) AS oi, 
 SUM( open_pull) AS op, 
 SUM( pull_review_comment) AS prc, 
 SUM( merge_pull) AS mp, 
 SUM( star) AS st, 
 SUM( fork) AS fo, 
  SUM(daily_score) AS actor_acitivity 
 FROM daily_score
WHERE repo_name='bitcoin/bitcoin'
GROUP BY actor_id
HAVING ic>0 AND oi>0 AND op>0 AND prc>0 AND mp>0 AND st>0 AND fo>0
ORDER BY actor_acitivity DESC
```

查询过如下：

![image](C:\Users\陈杰\OSSDevGov2021\final_repo_report\group_1\bitcoin数据类分析\查询结果.png)

将结果导出到excel后用python进行分析，代码如下：

```python
import pandas as pd
import math
sql_result = pd.read_excel("执行结果1.xlsx")
result_keys = list(sql_result.keys())
person_data = []
for i in range(len(sql_result)):
    Au = sql_result[result_keys[1]][i] + 2 * sql_result[result_keys[2]][i] + 3 * sql_result[result_keys[3]][i] + 4 * sql_result[result_keys[4]][i] + 5 * sql_result[result_keys[5]][i] 
    person_data.append(Au)
Adr = []
for i in range(len(person_data)):
    Adr.append(math.sqrt(person_data[i]))
RDPab = []
for i in range(len(Adr)):
    RDPab.append([])
    for j in range(len(Adr)):
        RDPab[i].append(0)
for i in range(len(person_data)):
    for j in range(len(person_data)):
        RDPab[i][j] = (person_data[i] * person_data[j]) / (person_data[i] + person_data[j])
RDGab = RDPab
```

输出结果如下：

![image](C:\Users\陈杰\OSSDevGov2021\final_repo_report\group_1\bitcoin数据类分析\输出结果1.png)

![image](C:\Users\陈杰\OSSDevGov2021\final_repo_report\group_1\bitcoin数据类分析\输出结果2.png)

