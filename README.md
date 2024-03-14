# Python-in-depth-from-scratch-to-learn-pandas
**_本仓库主要内容为本人自学李庆辉所著《深入浅出Pandas：利用Python进行数据处理与分析》时所记录的学习笔记与思考_**

目的在于：

  _1.帮助读者在自学此书时，解决运行代码时可能出现的一系列报错问题，达到畅通全书的作用_

  _2.帮助读者快速回顾pandas库中的重难点知识，并附带上了本人学习时总结出的一些小技巧_

---
1. 导入表格文件后，用df读取后输入df不显示结果——未安装excel可视化插件openpyxl 
2. 安装插件后继续输入df 仍然没有输出结果 显示df未定义——前面的import pandas as df未先行运行一次导致df未被定义 重新打开文件的时候 把必要的引入pandas和把文件读到df就行了
3. 读取excel数据时采用df.shape()查看行数和列数显示'tuple' object is not callable数组不可用——shape（）导致的问题 去掉括号即可
4. 查看信息 显示复杂的要加（） 普通的比如shape后面没有（）
5. 查看行的时候 df[x:x:x]这种 比如你用0 10 1 就是0到10每隔一个输出一个 结果就是10个都在(虽然0到10实际上是有11个的) 所以你不管0到几隔几个 因为你是0开头所以第一行它是会输出的 比如0 10 10 这时候就是只输出第一行了 因为你隔第十个已经是第11项了 如果只想要第一行和第十行就用df[0:10:9]这样隔到第9个的时候就是正好第十项 所以第一个x的地方是一定会输出的 也就是所选区域的第一行 肯定在
6. 输出前10行 可以df.iloc[:10:]也可以df.iloc[:10,:]
7. 通过df.loc['Eorge':'Alexander', 'team':'Q4'] 指定哪行的哪几列的时候 逗号前是制定哪行到哪行 逗号后是哪列到哪列  []里只能用一个逗号 不可以特指分开的哪两列   像df.loc[:,'Q1':'Q4'] 就是行全选 列只选Q1到Q4 这个后面会经常用到 因为有的列是字符串类型的 比如team和name 所以得把数字类型的挑出来 后面才好进行各种计算
8. df[df.team == 'C'] # team列为'C'的 这种查找C队的要加引号 但是如果是数字列的比如查Q1==90的就不用引号 数据类型是数字的时候可以不加引号 文本类型的时候要加引号查找
9. df[(df['Q1']>90)&(df['team']=='C')] 多重筛选的时候 df[]是作为一个整体后面跟大于等于小于的 这里的列名比如Q1和team要加引号
10. df[df['team']=='C'].loc[df.Q1>90] 多重筛选的时候 就不用加引号了 分析：df[]的格式要加引号 df.[]就不用了 尝试一下 用df[df['Q1']>90].loc[df.team=='C']前后换一下结果一样的
11. df.sort_values(by='Q1') 来做排序的时候 等号只要一个
12. 同时排序的时候 df.sort_values(['Q1','Q2'],ascending=[True,False]) 先排前面的 再排后面的 但是要注意这里不是将Q1整体从小到大排后Q2再整体从大到小排（实际上也不可能做得到这点 除非Q1Q2正好好相反）输出的结果是Q1先从小到大排 再对每一个Q1的值的小集合内部 再从大到小排一次Q2 相当于Q1有几个值 Q2就排了几次
13. df.groupby('team').sum())的时候 数字则把值相加 字符串则把字符串合并(把team列的值是一样的都加起来)
14. df.groupby('team').mean()的时候 因为别的列可能是字符串类型的 所以没法求平均会报错 所以得用df.groupby('team').agg({}) 来细化说明是哪几列求平均
15. 注意agg({})里说明要求count-计算总个数 和mean-求平均值的时候 这俩要带'' max min和sum不用 注意这里max、min、sum不带‘’的话 仍然可以输出正确结果 但是会显示warning 所以其实默认都带上‘’就好了
16. 把一个表后面加上.T 比如df.groupby('team').agg({'Q1':'sum','Q2':'count','Q3':'mean','Q4':'max'}).T 定义上是说沿左上到右下的对角翻转 不便理解 其实对应的地方的值不变 就是把列名跟行名互换了 翻转后行列交叉位置的值依旧对应原来行列交叉的地方的值
17.  df.groupby('team').agg({'Q1':'sum','Q2':'count','Q3':'mean','Q4':'max'}).stack() 这种的stack的功能就是原来你df.groupby('team').agg({'Q1':'sum','Q2':'count','Q3':'mean','Q4':'max'})这个表的行名不动 把每列的值给你罗列到一小团列出来 如果是df.groupby('team').agg({'Q1':'sum','Q2':'count','Q3':'mean','Q4':'max'}).unstack() 那就是把行列先换一下 然后再给你把一行中的每列的数据罗列到一小团起来
18. df['one']=1 这个增加是在最后再往右加一列 这里只用一个=
19. df['total']=df.loc[:,'Q1':'Q4'].apply(lambda x:sum(x),axis=1) 不知道啥意思 明天问ChatGPT（有时候不懂的基础语法直接问ChatGPT 回答相当棒）解答：loc这里是选了所有的行 选了Q1到Q4的列 apply是对选好的行进行运算的说明 lambda x是指每行中每个列所对应的那个值 意思就是你选的这个区域里的值 sum（x）表示对这里的值进行的是求和运算 axis=1的意思是这个函数将沿着行进行 即对每行 把Q1到Q4相加 axis=0则代表函数运算将沿列的方向进行 即竖着算到底
20. df['total'] = df.sum(axis=1) axis的作用已经知道是沿行的方向算了 但是运行df['total'] = df.sum(axis=1）会报错 说can only concatenate str (not "int") to str 这是因为axis加每一列的时候 有的列是字符串有的列是数字 sum函数对字符串是拼接 对数字是相加 但是字符串跟数字是没法加的 所以报错了 解决方法是不选整列 可以用df.loc[]选一下数字类型的列 也可以用numeric_columns = df.select_dtypes(include='number').columns选一下数据列 然后再df['total'] = df[numeric_columns].sum(axis=1)进行运算
21. df['avg']=df.total/4 添加一行平均值 也可以用df['avg']=df['total']/4
22. 因为作者给的表格里是有字符串数据的 所以直接进行df.mean() df.corr() df.count() 还有max、min、median、std（标准差）、var（方差）这种会报错 另外众数的格式比较特殊 是s.mode() 这时候用df.loc[]每步都先进行筛选再运算就有点麻烦了 可以用20条里说的定义来直接定义个只含数字类型的新表比如先定义 just_numeric_columns=df.select_dtypes(include='number').columns 再df[just_numeric_columns].mean()这种的就行了
23. df.mean()这种其实默认括号中是axis=0 也就是按列的方向计算 即计算每个竖着这一整列的均值 如果是df.mean(1) 那就是按行的方向计算 计算的是每行的这一横排的均值
24. 在 Pandas 中，CORREL 并不是一个内置的函数，所以在 Pandas 中使用 CORREL(Q1, Q2) 时会报错 可以用correlation = df['Q1'].corr(df['Q2'])来把Q1和Q2之间的相关系数存到correlation这个变量里
25. 之前10条中说df[]要加引号 df.[]不用加引号 这与24条吻合 可是为什么在22中的df[just_numeric_columns].mean()就不用加了？ 这是因为10条中这个规律适用于的是表格中的列 24中的Q1和Q2是代表列 可是22中的just_numeric_columns是一个全新的表格 并不是我们原来定义的df表格中的一个列名 所以并不冲突
26. df['Q1'].plot() 那么默认纵轴将是Q1这列的值 横轴是索引 即123456···
27. df['Q1'].plot()正常可以显示Q1一个图的 如果直接df.plot() 则会自动把所有四条数字类型的列画出来 会自动加颜色区分
28. df.loc['Ben', 'Q1':'Q2'].plot()报错说KeyError: 'Ben' 这是因为此时的df表格的索引为pandas自己默认加的索引0123456··· 而Ben这个人名在name列中，因此plot在索引中找不到Ben 而且也不可以在不想更改默认索引的话 用Ben所在行的默认索引来用于给plot定位 比如Ben在第一百行 默认索引是99 用df.loc['99', 'Q1':'Q2'].plot()和df.loc[99,'Q1':'Q2'].plot()仍然会报错KeyError 说明plot还是找不见你这个99在哪
29. 对于28条 解决办法是预先自设索引 把Ben所在的name列设为索引 采用df.set_index('name',inplace=True) 然后df.loc['Ben','Q1':'Q4'].plot()就可以正常输出了
30. 当28的情况正常运行后 这时候想以team列中的A队进行plot 依旧会报错KeyError 也就是找不到A 我的猜测是plot的定位必须是索引所在列 想要把某个位置对应行的数据plot出来 这个位置所在列就得是索引才能被plot查找到
31. 对于30条 用df.set_index('team',inplace=True)将索引换成team列后 再df.loc['A','Q1':'Q4'].plot()就会输出对应图像了
32. 但是注意 进行31条的操作后 此时打印df会发现此时team已经成了第一列 在原先表格中name列在team列前面 此时已经被删除了 也就是pandas默认我们选的索引将成为第一列 之前的列都会被删掉
33. 此时关于plot画图报错KeyError的问题已经全部解决了 那么 如果想把自己设置的索引取消掉怎么做呢？有两种方法 第一种是df.reset_index(inplace=True) 这里的inpalce=True表示这步reset索引的操作直接在原来的df上生效 此时打印df可以看到恢复成了默认的0～99索引 第二种是df.reset_index(inplace=False) 这个inplace=False的意思是不对原df修改 生成一个新的 重置为了默认索引的新df 因此直接运行df.reset_index(inplace=False)就会立刻生成这个新df的表格 而df.reset_index(inplace=True) 运行后则需要像往常一样继续输入df才能输出表格内容 同时 也可以采用df_new=df.reset_index(inplace=False)的方法把新的df存为df_new变量
34. 如果之前将name设为索引 然后又将team设为索引 此时就算reset重置了 也只是重置回将team设为索引之前的状态而已 name这列还是不在 想彻底恢复默认的话 可以直接重新用df=pd.read_excel('team.xlsx')导入一遍excel文件 这样就与原先一样了
35. 用df.groupby('team').sum().plot()画图的时候 比如行是ABCDE 列是Q1Q2Q3Q4 那就以列名分成几条线 然后图的横轴是行名 显示随行的变化 如果只想要代表Q1列的这条线的话 就用df.groupby('team').sum().Q1.plot() 即在plot前标明列名就行 不用带''   如果 想要展示随列的变化的话 plot前把表格转置就行 用df.groupby('team').sum().T.plot()
36. 用饼图表示Q1列中各组人数的对比时 用df.groupby('team').count().Q1.plot.pie()  这里要加Q1是因为Q1Q2Q3Q4每列的各组人数是一样的 只画一条就行了 然后饼图必须用plot.pie() 不能省掉plot不然会报错
37. df.to_excel('team-done.xlsx') 导出excel文件 df.to_csv('team-done.csv') 导出csv文件 导出文件的位置与打开的原表格文件在一起
38.  字符切片以左闭右开为原则
39. var[0:5:2]的时候 第一个也是默认取的
40. 用split分隔的时候 如果字符的第一个就是定义的分隔符 则在前面加一个' ' 如果前两个是连续的一样都是定义的分隔符 则两个间再加一个' '
41. '3月'.zfill(3)  后面括号中的3意味着长度是3个字符 不够的话加的时候在前面补0
42. a and b操作中 若a为假 则返回假的值（0）若a为真 返回b的值
43. a or b操作中 若有一个是假值 则返回真的值 若两个都为真 则返回a
44.  and的优先级高于or 所以无括号的情况下先进行and运算
