# 参数绑定和共享

有时我们需要一种方式来表达我们对模型参数适当值的先验知识。我们可能无法准确地知道应该使用什么样的参数，但我们根据领域和模型结构方面的知识得知模型参数之间应该存在一些相关性。

我们经常想要表达的一种常见依赖是某些参数应当彼此接近。考虑以下情形：我们有两个模型执行相同的分类任务（具有相同类别），但输入分布稍有不同。 形式地，我们有参数为 $$w^{(A)}$$ 的模型A和参数为 $$w^{(B)}$$ 的模型B。 这两种模型将输入映射到两个不同但相关的输出： $$\hat{y}^{(A)}=f(w^{(A)},x)$$ 和 $$\hat{y}^{(B)}=f(w^{(B)},x)$$ 。

我们可以想象，这些任务会足够相似（或许具有相似的输入和输出分布），因此我们认为模型参数应彼此靠近： $$\forall i$$ ， $$w^{(A)}_i$$ 应该与 $$w^{(B)}_i$$ 接近。我们可以通过正则化利用此信息。 具体来说，我们可以使用以下形式的参数范数惩罚： $$\Omega(w^{(A)},w^{(B)})=|w^{(A)}-w^{(B)}|_2$$ 。 在这里我们使用 $$L^2$$ 惩罚，但也可以使用其他选择。

正则化一个模型（监督模式下训练的分类器）的参数，使其接近另一个无监督模式下训练的模型（捕捉观察到的输入数据的分布）的参数。 这种构造架构使得许多分类模型中的参数能与之对应的无监督模型的参数匹配。

参数范数惩罚是正则化参数使其彼此接近的一种方式，而更流行的方法是使用约束：强迫某些参数相等。 由于我们将各种模型或模型组件解释为共享独立的一组参数，这种正则化方法通常被称为参数共享。和正则化参数使其接近（通过范数惩罚）相比，参数共享的一个显著优点是，只有参数（独立一个集合）的子集需要被存储在内存中。 对于某些特定模型，如卷积神经网络，这可能可以显著减少模型所占用的内存。

最流行和广泛使用的参数共享出现应用于计算机视觉的卷积神经网络中。 自然图像有许多统计属性是对转换不变的。 例如，猫的照片即使向右边移了一个像素，仍保持猫的照片。 CNN通过在图像多个位置共享参数来考虑这个特性。 相同的特征（具有相同权重的隐藏单元）在输入的不同位置上计算获得。 这意味着无论猫出当前图像中的第 $$i$$ 列或 $$i+1$$ 列，我们都可以使用相同的猫探测器找到猫。

参数共享显著降低了CNN模型的参数数量，并显著提高了网络的大小而不需要相应地增加训练数据。它仍然是将领域知识有效地整合到网络架构的最佳范例之一。

