<center> **<font size=6>De-Makeup Project Final Report</font>** </center>
<p align="right"> Team Fancy: Wuwei Cai, Jizhou Yang, Yufei Xie, Menglin Wang </p>



## Problem formulation

As the significant advances in the field of face recognition, Deep Network, like FaceNet [@FaceNet], performed almost perfectly in academics. However, we all have the experiences that your iPhone wouldn’t ‘know’ you for some reasons, especially for girls with makeup. Due to the noise of input pictures in  application scenarios, Face Recognition technique requires not only modeling network, which have been well developed, but also picture pre-processing algorithms.   
Compare to other noises that input images might have, makeup is a relatively frequent and formalized one. The applications of face recognition might benefit more from de-makeup technique. Therefore, our team is aiming to develop a network that can remove the makeup. What’s more, the de-makeup network itself is also a helpful and entertaining application.   
Due to the lack of aligned images (before and after makeup), this de-makeup problem is tackled with an unsupervised approach. We used cycle Generative Adversarial Network(cycle GAN), based on the framework of cycle-consistent generative adversarial networks, to produce de-makeup face images. The approach include two asymmetric networks: a forward network transfer makeup style, whereas a backward network removes the makeup.      
The network is trying to learn mapping functions between two domains, X and Y. The two mapping included in our model is $G: X \rightarrow Y$ and $F: Y \rightarrow X$. Two adversarial discriminators $D_X$ and $D_Y$ are also introduced, where $D_X$ is aiming to distinguish between images $\{ x \}$ and transformed images $\{ F(y)\}$ and $D_Y$ is aiming to distinguish between images $\{ y \}$ and transformed images $\{ F(x)\}$. The lost functions involved in our model can be defined as below:    

$$
\begin{aligned}
L_{GAN}(G,D_Y,X,Y) &= E_{y\sim P_{data}(y)}[logD_Y(y) ] + E_{x\sim P_{data}(x)}[log(1 -D_Y(G(x))) ]  \\\\\\
L_{cyc}(G,F) &= E_{x \sim P_{data}(x)}[\lVert F(G(x))-x\rVert_1] + E_{y \sim P_{data}(y)}[\lVert G(F(y))-y\rVert_1] \\\\\\
L_{full}(G,F,D_X,D_Y) &= L_{GAN}(G,D_Y,X,Y) + L_{GAN}(F,D_X,Y,X) + \lambda L_{cyc}(G,F) \\\\\\
G^\ast, F^\ast &= arg\ min_{G,F}\ max_{D_x, D_y}L_{full}(G,F,D_X,D_Y)
\end{aligned}
$$
<center>**Equation. 1 Loss function of cycleGAN [@CycleGAN2017]**</center>

## Approach

Our project can be roughly divided into three phase and within each pahse, we solved different problems.    
1) **Data collection and literature research:**    
 In the beginning of our project, we reviewed related paper and developed our method. In addition, we contacted the author of BeautyGAN [@BeautyGAN] and were authorized to use their large Makeup Transfer dataset. To enrich our training in advance, we also collected images manually, but due to the poor training performance, these data were discarded.    
2) **Implementation and training**    
After the settlement of method and data, we finished the implementation of cycleGAN network using Tensorflow, referring to the Zhu's paper[@CycleGAN2017] and some high-stared github respository, and trained on Google Cloud Platform.    
3) **Optimization of performance**    
In this process, we were iterating the process that updateing our method depending on feedbacks of training result and in the end, we increased our output performance significantly. In the process, we applied three solution helping with our performance. 1) After each iteration, we fine tuned the parameters to get better result. 2) We categorized our training dataset depending on skin and hair colors, restrcting the no-makeup domain andlimiting the features will be learned. 3) Given the makeup of eye region is different from lip region and skin region, a face parsing algorithm that segment face into component might be applied before training. However, in the origin paper [@CycleGAN], the author didn't describe the parsing algorithm in detail. We tried to develop our own parsing algorithm, but due to the limited time we have, we didn't finish the segmentation.    

## Comparision to the past works    
We found several literatures that dealing with the similar problem, makeup transfer and removal, for instance PairedCycleGAN [@CycleGAN], makeup detector and remover framework[@FaceBehind], and Image-to-Image Translation [@imagetranslation]. In the above literature, their work can produce impressive de-makeup face image, however, to our best knowledge, relative less investigator tried to make use of the de-makeup networks on top of face recognition application.    

## Results
<center class="third">
 <img src="https://raw.githubusercontent.com/weedkillers/Hello-World/master/vHX74-0.png" width = '150'>
 <img src="https://raw.githubusercontent.com/weedkillers/Hello-World/master/vHX74-1.png" width = '150'>
 <img src="https://raw.githubusercontent.com/weedkillers/Hello-World/master/vHX74-2.png"  width = '150'>
</center>

<center class="third">
<img src="https://raw.githubusercontent.com/weedkillers/Hello-World/master/vHX26-0.png" width = '150'><img src="https://raw.githubusercontent.com/weedkillers/Hello-World/master/vHX26-1.png" width = '150'><img src="https://raw.githubusercontent.com/weedkillers/Hello-World/master/vHX26-2.png"  width = '150'>
</center>
<center class="third">
    <img src="https://raw.githubusercontent.com/weedkillers/Hello-World/master/vHX16-O.png" width = '150'><img src="https://raw.githubusercontent.com/weedkillers/Hello-World/master/vHX16-1.png" width = '150'><img src="https://raw.githubusercontent.com/weedkillers/Hello-World/master/vHX16-2.png"  width = '150'>
</center> 



<center> **Figure. 1 Test Input images (left), primary result (middle) and final result (right)** </center>

As shown above, we can see that in our primary output, instead of transfering makeup, the network was actually transfering 'race' and in the meantime, keep the images aligned. Even thought this was an interesting application, it did reflected our one of our problem. The distribution of our training data is biased by the selection of images, which will make the mapping functions learned by our network odd and produce funny results. When we go back to our dataset, we found our data was biased in several ways and we improved the performance after data catorization and tuning.
1) Skin and hair color: Due to people from different race may have different skin and hair color, this will affect our model in many ways. For instance, the model might learn gold hair feature and applied it to a black hair girl, which will make it looks like our primary output. We categorized our training data by skin and hair color and as shown above, the performance was imporved.


2) Background: Background in the images always severe as noise and our network might transfer the backgound into abnormal color or mix it with hair. But we haven't developed an effective way to solve this problem.
</center><center class="third">
    <img src="https://raw.githubusercontent.com/weedkillers/Hello-World/master/vHX46-0.png" width = '150'><img src="https://raw.githubusercontent.com/weedkillers/Hello-World/master/vHX46-1.png" width = '150'><img src="https://raw.githubusercontent.com/weedkillers/Hello-World/master/vHX46-2.png"  width = '150'>
</center>
<center> **Figure. Test input image (left), and results with abnormal background (primary (middle) and final (right))** </center>
3) Other noise in image: Other noise might come from accessaries, glasses, hat or other object in the image.These images are only minor part of training dataset, but they will affect the performance a lot. After cleaned our training dataset, we can see the result is robust to these noise.

</center><center class="third">
    <img src="https://raw.githubusercontent.com/weedkillers/Hello-World/master/vHX19-0.png" width = '150'><img src="https://raw.githubusercontent.com/weedkillers/Hello-World/master/vHX19-1.png" width = '150'><img src="https://raw.githubusercontent.com/weedkillers/Hello-World/master/vHX19-2.png"  width = '150'>
</center>

<center class="third">
    <img src="https://raw.githubusercontent.com/weedkillers/Hello-World/master/vHX21-0.png" width = '150'><img src="https://raw.githubusercontent.com/weedkillers/Hello-World/master/vHX21-1.png" width = '150'><img src="https://raw.githubusercontent.com/weedkillers/Hello-World/master/vHX21-2.png"  width = '150'>
</center>
<center> **Figure. Test Input images with other noise (left), primary result (middle) and final result (right)** </center>

## References
