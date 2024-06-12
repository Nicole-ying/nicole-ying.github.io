+++
title = '生成对抗网络笔记'
date = 2024-01-08T14:16:21+08:00
draft = false
+++

>%matplotlib inline

%matplotlib inline: 用于在IPython环境（例如Jupyter Notebook）中内嵌绘制图表。它的作用是使得你创建的所有Matplotlib图表都会直接嵌入到Notebook中，而不是在新窗口中打开。

>plt.rcParams['figure.figsize'] = (10.0, 8.0) # set default size of plots
>
>plt.rcParams['image.cmap'] = 'gray'
>
>plt.rcParams['image.interpolation'] = 'nearest'

plt.rcParams 它是一个配置字典，允许全局自定义绘图的外观和行为。

plt.rcParams['figure.figsize'] = (10.0, 8.0)：设置了默认的图表大小。

plt.rcParams['image.cmap'] = 'gray'：设置了默认的颜色映射。颜色映射是一种将数值数据映射到颜色的方法。

plt.rcParams['image.interpolation'] = 'nearest'：设置了图像的插值方法。插值是在放大或缩小图像时使用的一种处理方法，用于估计新像素的值。

>images = np.reshape(images, [images.shape[0], -1])

将一个多维数组 images 重塑为一个二维数组，其中第一个维度为图片数量，第二个-1是自动计算维度使数组保持大小不变

>images.shape[0]

images 通常是一个三维或四维数组。例如，一个常见的四维数组形状是 (batch_size, height, width, channels)，在这种情况下，images.shape[0] 就是 batch_size，即批次中的图像数量。

>sqrtn = int(np.ceil(np.sqrt(images.shape[0])))

np.ceil：将输入的每个元素向上取整到最接近的整数。

np.sqrt：用于计算数组中每个元素的平方根。

>fig = plt.figure(figsize=(sqrtn, sqrtn))

plt.figure: 用于创建新的图形对象，以便在其上绘制图表。通过 figsize 参数来指定图形的宽度和高度，以英寸为单位。

>gs = gridspec.GridSpec(sqrtn, sqrtn)

gridspec.GridSpec(): 创建一个网格布局，该网格布局将被用于在图中安排子图。可以放sqrtn X sqrnt个子图。

>gs.update(wspace=0.05, hspace=0.05)

update方法来调整网格布局对象（gs）的间距参数。调整水平间距（wspace）和垂直间距（hspace）。

> for i, img in enumerate(images):

启动一个for迭代images列表。每次迭代，它将当前索引分配给变量i，并将当前图像分配给变量img。

>plt.axis('off')

plt.axis('off'): 会删除 x 和 y 轴上的数值以及刻度线，使绘图看起来像没有任何轴信息的干净图像。

>ax.set_aspect('equal')

用于保存图片缩放均衡，不变形。

>param_count = np.sum([np.prod(p.shape) for p in model.weights])

np.prod: 用于计算数组中元素的乘积。它接受一个数组作为输入，并返回数组中所有元素的乘积。

>numpy.prod(a, axis=None, dtype=None, keepdims=<no value>, initial=<no value>, where=<no value>)

a: 传入的数组。

axis: 沿着哪个轴相乘。

dtype: 指定数据类型。

keepdims：默认为false，设置为True则保存原来的维数不变，即沿着各轴相乘。

>answers = np.load('gan-checks-tf.npz')

np.load: 加载保存在文件.npz的数据，传给answers。

>train, _ = tf.keras.datasets.mnist.load_data()

返回训练集和测试集。

>X, y = train

X 通常表示输入特征（即图像数据），而 y 表示对应的目标标签。

>X = X.astype(np.float32)/255

将变量 X 中的数据类型转换为 np.float32。

/ 255：这一部分将图像数据中的像素值除以 255。这是一个常见的归一化操作，将像素值从范围 [0, 255] 缩放到范围 [0, 1]。这有助于模型的训练和稳定性，以及加速梯度下降的收敛。

>X = X.reshape((X.shape[0], -1))

X.shape[0]: 图片数量。

-1: 自动计算新的维度大小，以使数据被展平为一个一维数组，同时保留了图像数量信息。

>np.arange(4)

array([0, 1, 2, 3])

>np.arange(2, 10, 2)

array([2, 4, 6, 8])

从2到10，步长为2，输出不包括10

>np.random.shuffle(array)

用于随机打乱数组的元素的顺序

>for i in range(0, N, B)

这是一个 for 循环，用于遍历一个范围，其中 i 的起始值是 0，终止值是 N-1，步长为 B。这意味着 i 将从 0 开始，每次递增 B，直到达到 N-1 或 N。

>tf.nn.leaky_relu(x, alpha)

x：输入的张量或数值。

alpha：一个小的正数值，通常称为泄露系数。它控制了当输入小于零时，Leaky ReLU 允许一定程度的“泄露”，即允许一些负值通过。alpha 的值通常在很小的范围内，例如 0.01。

>tf.random.uniform(shape, minval=0, maxval=1, dtype=tf.float32)

用于生成服从均匀分布的随机张量。均匀分布指的是在指定的区间内，所有数值都具有相等的概率密度。

shape: 可以是[batch_size,dim]。

minval：这是均匀分布的下界，默认值为 0。

maxval：这是均匀分布的上界，默认值为 1。

>logits = tf.constant([0.5, -0.3, 0.8]) #模型的输出，未经过 softmax 激活
>
>labels = tf.constant([1.0, 0.0, 1.0]) # 真实标签数据
>
>bce_loss = tf.keras.losses.BinaryCrossentropy(from_logits=True) # 创建损失函数对象
>
>loss = bce_loss(labels, logits) # 计算损失
>
>print("Binary Crossentropy Loss:", loss.numpy())

损失函数的计算

>tf.constant 

用于创建一个常量值的张量，张量一被创建，将不能更改。

>constant_int = tf.constant(5, dtype=tf.int32) # 创建一个常量张量，包含整数值
>
>constant_float = tf.constant(3.14, dtype=tf.float32) # 浮点数值
>
>constant_string = tf.constant("Hello, TensorFlow!", dtype=tf.string) # 字符串
>
>constant_matrix = tf.constant([[1, 2, 3], [4, 5, 6]], dtype=tf.int32) # 多维数组
>
>constant_auto_shape = tf.constant([1, 2, 3]) # 形状自动推断
>
>constant_named = tf.constant(42, dtype=tf.int32, name="my_constant") # 指定名称

>with tf.GradientTape() as tape:

with 关键字创建了一个上下文，也就是一个代码块，在这个上下文中的操作将被 tape 记录下来。

tf.GradientTape()：用于跟踪计算过程中涉及的所有操作，以便后续计算梯度。

