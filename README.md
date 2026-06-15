# Tensorgrad 与张量手册

<div align="center">
<img src="https://raw.githubusercontent.com/thomasahle/tensorgrad/main/docs/images/basics.png" width="70%">
<h3>
  
[书稿](https://tensorcookbook.com) | [文档](https://tensorcookbook.com/docs) | [API 参考](https://tensorcookbook.com/docs/api)

</h3>
</div>

## Tensorgrad
一个张量与深度学习框架：像是 PyTorch 遇上 SymPy。

Tensorgrad 是一个用于符号张量操作的开源 Python 包。
它可以自动执行《Tensor Cookbook（草稿）》中描述的各种化简，也可以作为机器学习框架使用。

## 快速开始

通过 pip 安装 tensorgrad：

```bash
uv pip install tensorgrad
```

（可选）如果需要图示可视化（LaTeX/TikZ），请安装：

```bash
apt-get install texlive-luatex texlive-latex-extra texlive-fonts-extra poppler-utils
```

### macOS：PyTorch 编译问题

在 macOS 上，如果使用 PyTorch 的 `torch.compile` 功能时遇到 `'algorithm' file not found` 错误，需要改用 Homebrew 的 LLVM 编译器：

```bash
# 如果尚未安装 LLVM，请先安装
brew install llvm

# 使用正确的编译器运行 Python
export CXX=/opt/homebrew/opt/llvm/bin/clang++
python your_script.py
```

这个问题会影响使用 `torch.compile` 的脚本（例如带 PyTorch 后端的示例）。macOS 默认的 clang++ 缺少 PyTorch 编译器所需的 C++ 标准库头文件。

## 示例

如果想自己运行这些示例，可以使用[在线 playground](https://tensorcookbook.com/playground.html)，或查看[这个 notebook](https://colab.research.google.com/drive/10Lk39tTgRd-cCo5gNNe3KvdDcVP2F5aB?usp=sharing)。


### L2 损失的导数

```python
from tensorgrad import Variable
import tensorgrad.functions as F

# 定义张量边和变量的大小
b, x, y = sp.symbols("b x y")
X = tg.Variable("X", b, x)
Y = tg.Variable("Y", b, y)
W = tg.Variable("W", x, y)

# 定义误差
error = X @ W - Y

# Frobenius 范数的平方就是张量与自身所有边的缩并
loss = error @ error

# 计算损失关于 W 的导数
expr = tg.Derivative(loss, W)
```

这会输出如下张量图：

<img src="https://raw.githubusercontent.com/thomasahle/tensorgrad/main/docs/images/l2_grad_w_single_step.png" width="50%">

Tensorgrad 也可以输出用于数值计算关于 W 的梯度的 PyTorch 代码：
```python
>>> to_pytorch(grad)
"""
import torch
WX = torch.einsum('xy,bx -> by', W, X)
subtraction = WX - Y
X_subtraction = torch.einsum('bx,by -> xy', X, subtraction)
final_result = 2 * X_subtraction
"""
```

### CE 损失的 Hessian 矩阵

作为更复杂的例子，考虑下面这个用于计算交叉熵损失的 Hessian 矩阵的程序：

```python
# 将 logits 和 targets 定义为向量
i = sp.symbols("i")
logits = tg.Variable("logits", i)
target = tg.Variable("target", i)

# 定义 softmax 交叉熵损失
e = F.exp(logits)
softmax = e / F.sum(e)
ce = -target @ F.log(softmax)

# 通过对梯度再求梯度来计算 Hessian 矩阵
H = ce.grad(logits).grad(logits)

display_pdf_image(to_tikz(H.full_simplify()))
```

<img src="https://raw.githubusercontent.com/thomasahle/tensorgrad/main/docs/images/hess_ce.png" width="50%">

这是 `(diag(p) - pp^T) sum(target)` 的张量图记号，其中 `p = softmax(logits)`。

### 期望

Tensorgrad 还可以对任意函数关于高斯张量取期望。

例如，考虑前面的 L2 损失程序：
```python
# 定义张量边和变量的大小
b, x, y = sp.symbols("b x y")
X = tg.Variable("X", b, x)
Y = tg.Variable("Y", b, y)
W = tg.Variable("W", x, y)

# 定义分布的均值和协方差变量
mu = tg.Variable("mu", x, y)
# 矩阵的协方差是一个具有两个对称性的 4 阶张量
C = tg.Variable("C", x, y, x2=x, y2=y).with_symmetries("x x2, y y2")

# 假设 W ~ Normal(mu, C)，对 L2 损失取期望
l2 = F.sum((X @ W - Y)**2)
E = tg.Expectation(l2, W, mu, C, covar_names={"x": "x2", "y": "y2"})

display_pdf_image(to_tikz(E.full_simplify()))
```

<img src="https://raw.githubusercontent.com/thomasahle/tensorgrad/main/docs/images/expectation.png" width="50%">

注意，因为这里是对矩阵取期望，所以协方差是一个 4 阶张量（图中用星形表示）。
这不同于对向量取期望时得到的普通“矩阵形状”协方差。

### 求值

Tensorgrad 可以使用 [Pytorch Named Tensors](https://pytorch.org/docs/stable/named_tensor.html) 对图进行求值。
它使用图同构检测来消除公共子表达式。

### 代码生成

Tensorgrad 可以把图重新转换为 PyTorch 代码。
这为神经网络中的梯度和高阶导数提供了一种高度优化的计算方式。


### 矩阵微积分

在 Penrose 的著作 The Road to Reality: A Complete Guide to the Laws of the Universe 中，他引入了一种在张量网络上求导的记号。在这个库中，我们尽量遵循 Penrose 的记号，并按需扩展它，以处理张量函数上的完整“链式法则”。
<img style="background-color: white" src="https://upload.wikimedia.org/wikipedia/commons/thumb/3/37/Penrose_covariant_derivate.svg/2880px-Penrose_covariant_derivate.svg.png" width="100%">

另一个灵感来源是 Yaroslav Bulatov 对[神经网络 Hessian 矩阵的推导](https://community.wolfram.com/groups/-/m/t/2437093):

<img src="https://raw.githubusercontent.com/thomasahle/tensorgrad/main/docs/images/hessian_yaroslaw.png">

# 更多内容

## Transformers

<img src="https://raw.githubusercontent.com/thomasahle/tensorgrad/main/docs/images/attention.png">

## 卷积神经网络 

CNN 的主要组成部分是线性操作 Fold 和 Unfold。
Unfold 接收一张尺寸为 HxW 的图像，并输出 P 个大小为 K^2 的“patches”，其中 K 是卷积核大小。Fold 是反向操作。
由于它们都是线性操作（只包含复制/相加），我们可以把它们表示为形状为 (H, W, P, K^2) 的张量。

<img src="https://raw.githubusercontent.com/thomasahle/tensorgrad/main/docs/images/uCrOg.png" width="80%">

<a href="https://arxiv.org/abs/1908.04471">Hayashi et al.</a> 说明，如果定义张量 `(∗)_{i,j,k} = [i=j+k]`，那么 “Unfold” 算子会沿空间维度分解，而你可以很容易地把许多不同的卷积神经网络写成张量网络：
<img src="https://raw.githubusercontent.com/thomasahle/tensorgrad/main/docs/images/68747470733a2f2f64726976652e676f6f676c652e636f6d2f75633f6578706f72743d766965772669643d3141305235795371446e48715939624650677163546f6b347735416a516c666572.png">

使用 Tensorgrad，可以像这样写出“标准”卷积神经网络：
```python
data = Variable("data", ["b", "c", "w", "h"])
unfold = Convolution("w", "j", "w2") @ Convolution("h", "i", "h2")
kernel = Variable("kernel", ["c", "i", "j", "c2"])
expr = data @ unfold @ kernel
```

然后可以很容易地用 `expr.grad(kernel)` 符号化求出 Jacobian 矩阵：
<img src="https://raw.githubusercontent.com/thomasahle/tensorgrad/main/docs/images/conv_jac.png">

## Tensor Sketch

摘自[这条 Twitter thread](https://twitter.com/thomasahle/status/1674572437953089536)：
我当年研究 Tensor-sketching 时真希望自己已经知道张量图。
现在补上这一点，并用张量网络解释张量的降维：

<img src="https://raw.githubusercontent.com/thomasahle/tensorgrad/main/docs/images/ts_simple.png" width="66%">

第二个版本是 Rasmus Pagh 和 Ninh Pham 的“原始” Tensor Sketch。（https://rasmuspagh.net/papers/tensorsketch.pdf）每条纤维先通过一个 JL sketch 降维，然后结果逐元素相乘。
注意，为了得到相同的输出大小，每个 JL 的输出都比“simple” sketch 中更大。

<img src="https://raw.githubusercontent.com/thomasahle/tensorgrad/main/docs/images/ts_pp.png" width="66%">

接下来是我和合作者在 https://thomasahle.com/#paper-tensorsketch-joint 中提出的“recursive” sketch。
论文里我们有时把它描述成一棵树，但这其实并不重要。只是当我们意识到这一点时，树形图已经画好了。

<img src="https://raw.githubusercontent.com/thomasahle/tensorgrad/main/docs/images/ts_tree.png" width="66%">

AKKRVWZ-sketch 的主要问题在于内部使用了 3 阶张量，这比 PP-sketch 中的简单随机矩阵需要更多空间/时间。
我们可以把每个 3 阶张量替换成简单的 2 阶 PP-sketch，从而缓解这个问题。

<img src="https://raw.githubusercontent.com/thomasahle/tensorgrad/main/docs/images/ts_hybrid.png" width="66%">


最后，我们可以使用 FastJL 加速每一次矩阵乘法；FastJL 本身基本上就是一堆小矩阵的外积。不过到这一步，我的图已经开始有点过于复杂了。

## 另见

- [从 einsum 创建张量图的工具](https://thomasahle.com/blog/einsum_to_dot.html?q=abc,cde,efg,ghi,ija-%3Ebdfhj&layout=circo) 作者 Thomas Ahle
- [Ideograph: A Language for Expressing and Manipulating Structured Data](https://arxiv.org/pdf/2303.15784.pdf) 作者 Stephen Mell、Osbert Bastani、Steve Zdancewic

