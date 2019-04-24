# Final-Report

$$
\begin{aligned}
L_{GAN}(G,D_Y,X,Y) &= E_{y\sim P_{data}(y)}[logD_Y(y) ] + E_{x\sim P_{data}(x)}[log(1 -D_Y(G(x))) ]  \\\\\\
L_{cyc}(G,F) &= E_{x \sim P_{data}(x)}[\lVert F(G(x))-x\rVert_1] + E_{y \sim P_{data}(y)}[\lVert G(F(y))-y\rVert_1] \\\\\\
L_{full}(G,F,D_X,D_Y) &= L_{GAN}(G,D_Y,X,Y) + L_{GAN}(F,D_X,Y,X) + \lambda L_{cyc}(G,F) \\\\\\
G^*, F^* &= arg\ min_{G,F}\ max_{D_x, D_y}L_{full}(G,F,D_X,D_Y)
\end{aligned}
$$

$$ \begin{aligned}L_{GAN}(G,D_Y,X,Y) &= E_{y\sim P_{data}(y)}[logD_Y(y) ] + E_{x\sim P_{data}(x)}[log(1 -D_Y(G(x))) ]\end{aligned} $$
$$ \begin{aligned} L_{cyc}(G,F) &= E_{x \sim P_{data}(x)}[\lVert F(G(x))-x\rVert_1] + E_{y \sim P_{data}(y)}[\lVert G(F(y))-y\rVert_1]\end{aligned}$$
$$ \begin{aligned}L_{full}(G,F,D_X,D_Y) &= L_{GAN}(G,D_Y,X,Y) + L_{GAN}(F,D_X,Y,X) + \lambda L_{cyc}(G,F)\end{aligned}$$
$$ \begin{aligned} G^*, F^* &= arg\ min_{G,F}\ max_{D_x, D_y}L_{full}(G,F,D_X,D_Y)\end{aligned}$$
