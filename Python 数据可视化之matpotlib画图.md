### Python 数据可视化之matpotlib画图

#### 目录

##### 一、建立画布和坐标系

##### 二、解决中卫乱码问题

##### 三、介绍多种绘图方法以折线图为例

##### 四、多种图形的绘制方法

1、折线图

2、条形图

3、面积图

4、填图

5、饼图

6、直方图和核密度图

7、散点图

8、箱线图

9、雷达图

##### 五、图形保存

##### 六、拓展图形







##### 一、建立画布和坐标系

```python
#导入绘图所用的相关库 
import numpy as np 
import pandas as pd 
import matplotlib as mpl 
import matplotlib.pyplot as plt
```

```python
#方法1
fig=plt.figure(figsize=(10,5),edgecolor='r') # figsize参数，设置画布大小，edgecolor参数，设置画布边框颜色
# 在画布上添加坐标系
ax1=fig.add_subplot(1,2,1)   # 1行2列，第一个子坐标系

#方法2  同时建立figure和坐标系
fig1,axes=plt.subplots(nrows=2,ncols=2,figsize=(7,5))
```

##### 二、解决中卫乱码问题

```python
plt.rcParams['font.sans-serif']='SimHei'
plt.rcParams['axes.unicode_minus']=False
```

##### 三、介绍多种绘图方法以折线图为例

```python
# 直接用数据框实例的plot方法
df1=pd.DataFrame(np.random.randn(30,3),columns=list("abc")).cumsum() # cumsum 累计求和
# 单个变量折线图
df1['a'].plot.line(color='r',linestyle='--',marker='^',alpha=0.5)  # 颜色=k 代表黑色,'--' 表示虚线 ，'-'实线， 

# 多变量折线图方法1
df1.plot.line(colormap='summer',linestyle='--',marker='.',grid=True) # 多变量时给调色板colormap
# 多变量折线图方法2,画折线图line可以省略，可以指定颜色
df1.plot(kind='line',y=['a','b'],marker='+',linestyle='-',color=['r','b'])
```

```python
# 画图的时候指定坐标系
fig1,axes=plt.subplots(2,1,figsize=(6,10))
df1.plot(kind='line',colormap='spring',marker='.',
         linestyle='--',grid=True,alpha=0.8,ax=axes[0])
         
# 先生成坐标系,再在指定坐标系上画图
fig1,axes=plt.subplots(1,2,figsize=(10,5))
df1=pd.DataFrame(np.random.rand(10,3),columns=list('abc')).cumsum()
df2=pd.DataFrame(np.random.rand(10,3),columns=list('abc')).cumsum()
axes[0].plot(df1,linestyle='--',marker='.',alpha=0.5) # 和上面方法对比，不能设定grid参数
axes[1].plot(df1,'.--',df2,'+-')
```

```python
#坐标系基本元素的设定
ax.set_title('时间序列图',fontsize=20)  # 添加图标题  fontsize参数设置字号大小
ax.set_xlabel("时间",fontsize=30) #设定x轴标签 
ax.set_ylabel("值",fontsize=30) #设定y轴标签 
ax.set_xlim([0,60])  # x轴边界 
ax.set_ylim([-20,15])  # y轴边界 
h1=range(0,60,5)   
ax.set_xticks(h1)     # 设置x刻度 
ax.set_xticklabels(h1) # x轴刻度标签 
v1=range(-20,15,4)
ax.set_yticks(v1)  # 设置y刻度 
ax.set_yticklabels('%.2f'%i for i in v1)   # y轴刻度标签 
ax.grid()    # 添加网格线
ax.legend(loc='upper left')  # 显示图例，loc表示位置 
ax.figure  #查看图
```

```python
#在某个坐标轴的指定位置添加文字注释
ax.text(10,11,'Hello!',fontsize=10,color='r',rotation=45,va='bottom')  # 在图上添加'Hello'字符
fig1.suptitle('这就是我的画布',fontsize=25)     # 添加超级标题
```

![可视化1](D:\学习知识整理\Python\图片\matplotlib可视化\可视化1.png)

##### 四、多种图形的绘制方法

1、折线图

```python
# 简洁方式
df1.plot(style={'x1':'ro--','x2':'b+--','x3':'k.-'})
# 正常方式，参数可省略
# s.plot(style="--.",alpha = 0.8,colormap = 'Reds_r',mark='.',grid=True) 
```

2、条形图

```python
fig,axes=plt.subplots(5,1,figsize=(10,20)) 
#2单变量画图 纵向条形图 
df1['x1'].plot.bar(color='b',alpha=0.7,grid=True,ax=axes[0])
axes[0].set_title('单变量纵向条形图')   # 添加标题
#3单变量画图 横向条形图
df1['x1'].plot.barh(edgecolor='#00FFFF',facecolor='r',
                    alpha=0.5,width=0.5,ax=axes[1]) # width 柱子宽度
axes[1].set_title('单变量横向条形图')
# 4多系列柱状图 
df1.plot.bar(colormap='winter',ax=axes[2])
axes[2].set_title('多变量条形图')
# 5多序列堆积图
df1.plot.bar(stacked=True,ax=axes[3])
axes[3].set_title('堆积图')
# 6画两个单变量的图 
df1['x1'].plot.bar(color='r',ax=axes[4])
df1['x2']=-df1['x2']
df1['x2'].plot.bar(color='b',ax=axes[4])
for i,j in zip(df1.index,df1['x1'].values):
    axes[4].text(i,j,'%.2f'%j,va='top',ha='center',color='k')  # 在柱子上添加数值
for i,j in zip(df1.index,df1['x2'].values):
    axes[4].text(i,j+0.1,'%.2f'%-j,va='bottom',ha='center',color='w') 
axes[0].figure
```

![1567425444479](D:\学习知识整理\Python\图片\matplotlib可视化\可视化2.png)

3、面积图

```python
df1.plot.area(colormap='summer',stacked=True)
```

4、填图

```python
fig,axes=plt.subplots(4,1,figsize=(8,15))
x=np.linspace(0,9.42,100)
y1=np.sin(x)
y2=np.cos(x)
axes[0].plot(x,y1,label='sinx')
axes[0].plot(x,y2,label='cosx')
axes[0].legend()
axes[0].set_title('折线图')
axes[1].fill(x,y1,'r',alpha=0.5,label='sinx')
axes[1].set_title('填图1')
axes[2].fill(x,y2,'b',alpha=0.5,label='cosx')  
axes[2].set_title('填图2')
axes[3].fill_between(x,y1,y2)
axes[3].settitle('填图3')
```

5、饼图

```python
fig,axes=plt.subplots(1,2,figsize=(15,7))
df1=pd.DataFrame(np.random.rand(5,3),columns=['x1','x2','x3'],index=list('abcde'))
df1['x1'].plot.pie(colormap='winter',ax=axes[0])
df1['x2'].plot.pie(explode=[0.1,0,0,0,0],radius=1,ax=axes[1], 
                   autopct='%.2f%%',textprops={'fontsize':20},  
                  labeldistance=1.3)    
axes[1].set_ylabel(None)  # 删除y轴标签
axes[1].set_title('饼图',fontsize=25)  
```

```python
#  pie参数解释
#explode：指定每部分的偏移量（通过对这个参数的设定可以使得指定扇形与原点分离）； 
#labels：标签； 
#colors：颜色； 
#autopct：每个扇区值标签的显示方式； 
#pctdistance：扇区值标签的相对位置,是相对于半径而言的。默认值是0.6； 
#labeldistance：每个扇形标签的（放射状）位置,默认值：1.1。这个值是半径的倍数的意思； 
#shadow：阴影； 
#startangle：开始角度； 
#radius：半径；
#frame：图框； 
#counterclock：指定指针方向，顺时针或者逆时针； 
#一些英文单词：radial----放射状的 辐射状的，
```

![1567425320654](D:\学习知识整理\Python\图片\matplotlib可视化\可视化3.png)

6、直方图和核密度图

```python
fig,axes=plt.subplots(3,1,figsize=(6,18))
df1=pd.DataFrame({'a':np.random.normal(5,2,1000),
                 'b':np.random.normal(7,2,1000),
                 'c':np.random.normal(9,2,1000),
                 't':np.random.randint(1,4,1000)})
df1.plot.hist(ax=axes[0],alpha=0.8)
df1['a'].plot.hist(ax=axes[1],density=True,color='b',alpha=0.5,bins=50)
df1['a'].plot.kde(style='r--',ax=axes[1])  # 核密度图
```

![1567425286387](D:\学习知识整理\Python\图片\matplotlib可视化\可视化4.png)

7、散点图

```python
fig,axes=plt.subplots(4,1,figsize=(5,20)) 
df1=pd.DataFrame({"x":np.random.randn(1000),                  
                  "y":np.random.randn(1000),                  
                  "z":np.random.randn(1000)}) 
axes[0].scatter(df1["x"],df1["y"],color="b") 
axes[0].grid()
axes[1].scatter(df1['x'],df1['z'],color='r',alpha=0.5)
axes[1].grid()
axes[1].scatter(df1['x'],df1['y'],color='g',alpha=0.5)
axes[1].legend()
temp=axes[2].scatter(df1['x'],df1['y'],c=df1['y'],cmap='Greens',alpha=0.5)
fig.colorbar(temp,ax=axes[2])  #添加色柱
df1.plot.scatter(x="x",y="y",                  
                 c=df1["y"],cmap="summer",ax=axes[3],colorbar=True) # 数据框的下可以直接添加色柱
```

![1567425153129](D:\学习知识整理\Python\图片\matplotlib可视化\可视化5.png)

8、箱线图

```python
fig,axes = plt.subplots(2,1,figsize=(10,20)) 
df1 = pd.DataFrame({"x1":np.random.normal(0,1,1000),                                         
                    "x2":np.random.normal(0,1.5,1000),                                                           
                    "x3":np.random.normal(1,1,1000),                    
                    "x4":np.random.normal(1,2,1000)}) 
color = dict(boxes='black', whiskers='yellow', medians='DarkBlue', caps='red') 
# 方法1
df1.plot.box(color=color,ax=axes[0])
df1[['x1','x2']].plot.box(color=color,vert=True,ax=axes[1],grid=True,sym='r+')

#方法2
fig,ax = plt.subplots(1,1,figsize=(10,6))
f=df1.boxplot(sym="o",
           showbox=True,
           showmeans=True,
           showcaps=True,
           patch_artist=True,
           return_type="dict",
           notch=True,ax=ax)
```

9、雷达图

```python
fig=plt.figure(figsize=(8,8))
ax=fig.add_subplot(111,polar=True)
labels=['语文','数学','英语','物理','化学','生物']
zhangsan=np.array([90,80,78,78,89,45])
lisi=np.array([34,78,67,76,87,67])
angles=np.linspace(0,2*np.pi,6,endpoint=False)

zhangsan=np.concatenate((zhangsan,[zhangsan[0]]))
lisi=np.concatenate((lisi,[lisi[0]]))
angles=np.concatenate((angles,[angles[0]]))
ax.plot(angles,zhangsan,'ro--',label='张三')
ax.plot(angles,lisi,'bo--',label='李四')
ax.fill(angles,zhangsan,facecolor='r',alpha=0.2) #  填充
ax.fill(angles,lisi,facecolor='b',alpha=0.2)
angles1=angles*180/np.pi   #先要进行换算,弧度制变成角度制
ax.set_thetagrids(angles1,labels) # 添加标签
ax.set_title("分数雷达图") 
ax.grid(True) 
ax.legend(loc = (-0.2,1))  #用这个命令可以让图例和雷达图不重合在一起 ,第一个数字是水平位置，第二个数字是垂直位置，
```

![1567425096972](D:\学习知识整理\Python\图片\matplotlib可视化\可视化6.png)

##### 五、图形保存

```python
fig.savefig('mypicture.jpg')
```

##### 六、拓展图形

```python
#参数，start，stop，num（点的数量） 
xv=np.linspace(-10,10,100) 
yv=np.linspace(-10,10,100) 
x,y=np.meshgrid(xv,yv) 
x=x.ravel()
y=y.ravel()

#收集符合条件的点 
xc=x[(x**2+y**2)**0.5<=10] 
yc=y[(x**2+y**2)**0.5<=10]
fig,ax=plt.subplots(1,1,figsize=(10,10)) 
ax.scatter(xc, yc, s=5, c=xc, cmap="cool")
```

![1567425036500](D:\学习知识整理\Python\图片\matplotlib可视化\可视化7.png)