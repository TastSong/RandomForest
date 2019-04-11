# RandomForest

## 一．算法流程：

1. 读入训练集trainset,测试集testset

2. 将训练集分割为输入trainIn,输出trainOut

3. 这里假设类别数N为26，将trainOut[记录条数] 映射为 transformTrainOut[训练记录数][26]

4. 初始化transformTestOut[测试记录数][26]全部为0

5. For i = 1 : ForestSize:

​      //对训练集采样，这里要注意输入和输出一致

​      [sampleIn,transformSampleOut] = TakeSample(trainIn,transformTrainOut) 

​      For category = 1 : 26:

​           //CartTree 数组存放着26棵二分类树

​           CartTree[category]  =  TrainCartTree(sampleIn,transformSampleOut);

​      end

​      //transformTestOut[测试记录数][26]为承接二分类树输出的容器

​      for  i1 = 1 : testSetNum:

​          For category = 1 : 26:

​              transformTestOut[i1][category]  += predict(CartTree[category],testset[i1])

​         end

​      End

End

6. 遍历 transformTrainOut[]，将其每一行的最大值的下标作为该行记录的索引值。

 

## 二．决策树及随机森林的配置

1. 决策树

​     在这里，我们每一次26分类是由26棵CART共同完成的，CART的cost function采用的是gini系数，CART的最大层数为7，分裂停止条件为当前节点GINI为0或者当前节点所在层数到达了7.

2. 随机森林

a.随机森林每次循环的训练集采样为原训练集的0.5.

b.对于森林中每一棵决策树每一次分割点的选取，对属性进行了打乱抽样，抽样数为25，即每次分割只在25个属性中寻找最合适的值。并且对于每个选取的属性，我们进行了行采样。即如果这个属性所拥有的属性值数大于30，我们选取其中30个作为分割候选，如果小于30，则全部纳入分割候选。