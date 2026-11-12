# 县域校舍与教室配置

estate 校验地区、学段、楼宇、教室与通勤关系；allocation 求整数分配并生成校验凭据；assessments 比较供给与干预；runs 管理输入、版本、导出及恢复。

同一时段教室只能承担一个学段，楼宇座位上限由多个教室共享。最低保障、教室面积与通勤阈值均为硬约束，达到搜索边界不代表已证明最优。记录为合成校舍与学生需求。

## 运行

Python 3.11，使用标准库及内置 SQLite，无第三方依赖。无需安装第三方包。 在项目根目录执行，命令不访问公网。演示在临时目录内创建数据，结束后自行清理，不需要预置凭据或另启服务。

```sh
python3 estate_cli.py demo --fixture fixtures/county.json
```

## 开发检查

测试覆盖当前产品的核心计算、边界输入、事务或版本恢复及异常路径。编译或语法检查可单独运行。

```sh
python3 -m unittest discover -s tests -v
python3 -m compileall -q estate allocation assessments runs estate_cli.py
```

## 容器复现

先在有网络的准备阶段取得运行镜像。之后可在断网容器中复制项目到临时工作区运行，避免向源目录写入缓存。

```sh
docker build -t local/analysis-product .
docker run --rm --network none local/analysis-product
```

具体接口参数可通过主入口的帮助命令查看。数据库、归档和导出文件应存入调用者指定的工作目录，不提交到源码仓库。
