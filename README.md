针对 Stable Diffusion 提出高效水印算法
1.引言
生成模型是机器学习领域的一个重要分支，它的目标是生成具有真实性的图像、文本或其他数据。在过去的几年里，生成模型取得了巨大的进展，其中深度卷积生成对抗网络（DCGANs）、变分自编码器（VAEs）、生成对抗网络（GANs）等模型引领了潮流。然而，训练生成模型仍然面临一些挑战，如训练不稳定、模式崩溃等问题。Stable Diffusion 的出现旨在解决这些问题。Stable Diffusion 是一个强大的生成模型训练工具，它在机器学习领域引起了广泛的关注和研究。该工具的核心思想是通过稳定的梯度流来训练生成模型，从而提高生成图像和数据的质量。生成式人工智能模型可以生成极其逼真的内容，这对信息的真实性提出了越来越大的挑战。 为了应对这些挑战，人们利用水印来检测人工智能生成的内容。 具体来说，在发布人工智能生成的内容之前，将水印嵌入到该内容中。 如果可以从中解码出类似的水印，则将内容检测为 AI 生成。【1】对生成模型的输出加水印是追踪版权和防止人工智能生成内容潜在危害的一项关键技术。但是在实际的检测工作中遇到了很多问题，即使在对生成式模型生成的图像加上水印的情况下，水印仍可以在保持图像质量不变的情况下被扰动，最终导致现有的大部分水印检测存在误判等不足。【2】所以当前很有必要提出一种更加高效的水印算法来在生成式模型生成图像的过程中嵌入水印，并能够通过预先训练的水印提取器从任何生成的图像中恢复隐藏的签名，然后进行统计测试以确定它是否来自生成模型。
2.研究背景以及意义
研究背景：
人工智能和计算机视觉领域的研究领域中，AI绘画是研究的热点之一，因为大自然中的景物形态万千，运动不规则，且具有一定的生命周期，因此用计算机进行绘制和描述是很困难的。AI绘画作为一种新兴的艺术形式，虽然展现了巨大的潜力和魅力，但也不可避免地遇到了一些挑战和问题，需要我们认真地思考和解决。在这一部分中，我们将重点研究AI绘画所面临的主要挑战--版权归属，并提出可能的解决方案或建议。当一个AI程序根据用户输入的文本生成一幅图像时，这幅图像的版权归属在不同国家和地区可能有不同的解释和处理方式。【3】目前市面上用的最多的AI绘画工具是Stable-Diffusion（SD）和Midjourney（Mid）。这两种AI绘画工具都是使用深度学习生成的模型，具有一定的“创作”能力，但不可避免在版权方面遇到了问题和挑战。对于版权归属的问题，我们可以将AI生成的图像分为原创作品、衍生作品、公共领域作品等，目前较为合理的办法是对生成的图像添加水印，利用水印中的二进制信息来存储并明确各自的版权归属者、使用者、义务者等。优异的人工智能模型不仅需要设计者的智慧,而且需要消耗大量的训练数据和计算资源。因此,人工智能模型作为-种昂贵的数字资产,如何保护其知识产权不受侵害十分重要。
研究人工智能模型的知识产权保护具有显著意义。主要体现在: 1) 该研究领域方兴未艾,其基础理论与关键方法中蕴含重要的科学问题,极具学术研究价值; 2)对人工智能模型标识所有者、使用者、版本号、传播路径并进行篡改检测，建立人工智能模型全生命周期的秩序保障，可为人工智能的发展和应用提供必不可少的良好环境; 3) 该研究是利用人工智能技术保护人工智能模型，其成果将辐射和带动人工智能安全技术进步。【4】



3.国内外发展现状
如今，关于人工智能模型水印研究进展主要分为五种，分别为如下：白盒模型水印，黑盒模型水印， 无盒模型水印， 无盒模型水印

……


3.1 国外发展现状

最初提出的是白盒模型水印，白盒水印允许水印提取者完全知悉目标模型的内部细节，这为水印的验证提供了巨大的便利条件。2017 年，Uchida 等人（2017）首次提出了一种将数字水印技术用于神经网络模型版权保护的白盒方案。Tartaglione 等人（2021）提出白盒模型零水印方案，在模型训练前随机选取模型中的部分权重并进行标记，利用约束条件确保该部分权重在训练过程中不伴随梯度下降而进行权值更新。随着受保护模型类型的扩展，白盒模型水印的应用范围也在拓宽。Chen 等人（2021b）依据“彩票假设”提出一种针对“中奖彩票”类稀疏网络的版权保护方案。从目前已报导的研究成果数量上看，国内外学者在白盒模型水印方面呈现了并驾齐驱的竞争态势。

黑盒模型水印的基本框架也是由国外学者率先提出，Face⁃book团队的 Adi等人（2018）提出了第一个基于后门攻击的黑盒模型水印方法。为了提高鲁棒性，美国杜克大学的Jia等人（2021b）通过使用纠错码使水印提取过程更加鲁棒；新加坡南洋理工大学的 Chattopadhyay 和 Chattopad⁃hyay（2021）通过固定网络层微调的方法使网络的每一层都学习对应的触发样本，从而使简单的模型修改无法擦除嵌入的后门水印。还有Facebook、IBM等知名科技公司的研究团队，研究成果引起了学术界和工业界的广泛关注。

脆弱模型水印，目前，脆弱模型水印方面的研究成果还相对较少。国外代表性的研究成果包括美国普林斯顿大学提出的基于敏感样本的脆弱水印、意大利都灵大学提出的具有篡改定位功能的脆弱水印、美国蒙特克莱尔州立大学提出的外包模型完整性验证和隐私保护以及美国克莱门森大学提出的能够从原始的预训练模型生成带有不同水印的受保护模型等。研究人员还研究了结合频域信息隐藏的模型脆弱水印算法（Abuadbba等，2021），以检测数据投毒、微调以及后门等攻击。



3.2 国内发展现状

白盒模型水印，为了提升水印的隐蔽性和鲁棒性，Wang 等人（2020）提出基于反向传播实现的模型水印方案。Fan 等人（2022）强化了护照学习任务和原始任务之间的耦合关系，但造成使用方面的局限性。并且，方案涉及了对目标模型结构的修改，对目标模型的性能造成一定影响。因此，Zhang等人（2020b）提出了不改变模型结构的改进方案。通过在护照层中添加支持护照识别的额外分支，模型所有者只将不带护照层的目标模型交付给终端用户进行使用，并保存这些秘密护照和额外添加的护照层。所有权验证阶段，将护照层插入可疑模型的相应位置，如果提供护照为真，模型性能将保持不变。

黑盒模型水印，国内学者在黑盒模型水印的研究方面做出了相应的贡献。山东大学的 Li 等人（2019）使用深度隐藏的方式，将版权信息嵌入在原始输出中以生成对应的触发图像，并约束触发图像和原始干净图像分布相似，进一步提升了触发图片的隐蔽性。。为了更好地满足保真度的要求，南京航空航天大学的Sun等人（2021）使用了额外的类别作为触发类别，但其选取与原始数据分布无关的数据作为触发数据以及基于最低有效比特位的身份信息嵌入策略，无法很好地满足隐蔽性和鲁棒性的要求。百度团队的 Yang 等人（2021）将黑盒模型水印的训练过程转化为一个双层优化问题，目的是找到和模型紧密绑定的验证样本以及使用尽可能少的权值改动来记住这些验证样本，相比之前的方法，在保真度和鲁棒性上都有了相应的提升。为了同时满足触发图像隐蔽性和鲁棒性方面的需求，中国科学技术大学的Zhang等人（2022a）使用图像的边缘结构作为触发模式，并提出了名为“毒墨水”的触发图像生成策略。传统的黑盒模型水印基于异常标签的后门攻击方法，但是在为模型嵌入水印的过程中，也会引入相应的安全性问题。样为了解决上述安全性问题，清华大学的Li等人（2022c）使用无目标的后门攻击方法进行水印嵌入，削弱了特定触发模式和目标标签之间的联系。

脆弱模型水印国内学者在脆弱模型水印方面也取得了一些进展。例如，在基于敏感样本的脆弱模型水印方面，电子科技大学的Xu等人（2020a）提出了一种新的敏感样本生成方法，通过结合同态加密实现了隐私保护验证，可以高精度地验证外包给服务器的模型参数的完整性。复旦大学的Zhu等人（2021）提出了一种基于触发集的脆弱模型水印算法。该算法随机产生一些触发图像，并根据密钥标注对应的标签，从而生成触发集。为了避免对原始模型造成这种永久性扰动，中国科学技术大学的 Guan 等人（2020）提出了一种基于可逆水印的模型完整性认证算法，，在水印嵌入时采用了可逆信息隐藏中经典的直方图平移算法确保了可逆性。Zhao 等人（2022）提出一种自嵌入脆弱模型水印方案，不仅能识别和定位模型中被篡改的参数块，还能准确地恢复被破坏的参数。在不影响模型的任务功能的条件下，实现了篡改定位检测和参数恢复功能。

4.研究内容与方法
4.1研究内容
    本毕业设计的主要研究内容分为以下几个部分
    1.对diffusion->latent diffsion->stable diffusion一系列生成模型训练工具进行原理学习以及源码参考
    2.对生成训练工具中提到的多种水印方式进行原理掌握以及学术先进成果学习与复现
    3.对较为关键的部分代码获解码器进行适当微调进而进行水印植入乃至提高水印安全性以及检测精度
    4.研究如何合理调整参数和训练策略来实现较好的文本生成图像和水印的效果

4.2拟采用的方法
    针对4.1小节中的几个研究内容，分别采用以下研究思路和方法……
    1.对多种生成训练模型工具环境的搭建以及模型的使用，参数的调整和良好的训练策略
    2.对多种水印工具原理的学习和运用，包含目前主流的水印生成算法以及水印算法在生成式模型中的导入和使用。
水印算法的具体操作：
水印预训练
联合训练简单的卷积神经网络，方法与 HiDDeN 相同。
在主体框架生成图像后使用水印编码器对图像进行水印处理并生成新的图像，生成的新图像经过裁剪、翻转或JPEG 压缩后生成新的图像。再使用水印提取器将强化过的图像进行水印提取，然后进行水印的训练和分析。
微调LDM解码器
然后，我们将 VAE 的解码器 D 微调为 K，以生成隐藏固定签名 M 的图像（请注意，所有网络都是固定的，除非K）。 这是受到 LDM 训练过程的启发。 在微调循环期间，对一批 x 训练图像进行编码，然后由 D 进行解码，得到图像 x1。 然后将要最小化的损失计算为消息损失 L 和感知图像损失 Li 的加权和，消息损失 L 旨在最小化提取的消息 m1 和目标消息 m 之间的距离，感知图像损失 Li 旨在生成接近目标消息的图像。
统计检验
一旦我们检索到签名m'从图像2，我们使用统计测试来确定图像是否由我们的一个模型生成。这给出了误报率的理论伦理值，即错误检测
由我们的模型之一生成的图像的概率。我们将使用匹配位数M(m,m在提取的消息之间m和签名m的模型来测量两条消息之间的距离。检波
我们假设 Alice 已经嵌入了一个k-bits 签名m在解码器中D我们检验统计假设H1:"2由Aice的模型生成”反对原假设H 不是由 Alice 的模型生成的.我们计算匹配位数M之间m :从中提取的消息和签名m.我们决定标记图像2由 Alice 的模型生成，如果M大于闽值7.
检测的 FPR 


5.参考文献
1.Evading Watermark based Detection of AI-Generated Content
Zhengyuan Jiang, Jinghuai Zhang, N. Gong
2023, arXiv.org
2.
Improving Synthetically Generated Image Detection in Cross-Concept Settings
P. Dogoulis+ 2 authorsS. Papadopoulos
2023, MAD@ICMR
3.探析当下AI绘画的困境
4.人工智能模型水印研究进展
