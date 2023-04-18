---
title: 基本統計
categories: 
- [kalman]
math: true
---
若 $\mathbf{x}$是由實數$x_1$、$x_2 \cdots x_n$ 構成的離散隨機變數。則
* 均值(Mean) $\mu$: 一組數據的均衡點
$$
\mu = \frac{1}{n}\left.( x_1 + x_2 + \cdots + x_n\right.)
$$
* 標準差(Standard Deviation) $\sigma$: 一組數值的離散程度
$$
\sigma = \sqrt{\frac{1}{n}\left.[ (x_1-\mu)^2+(x_2-\mu)^2+\cdots+ (x_n-\mu)^2\right.]}
$$
* 變異數、方差(Variance) $\sigma^2$: 同樣是評估一組數值的離散程度
$$
\sigma^2 = \frac{1}{n}\left.[ (x_1-\mu)^2+(x_2-\mu)^2+\cdots+ (x_n-\mu)^2\right.]
$$
注:常用的符號表示有 $\mathrm{Var(\mathbf{x})}$ 、 $s^2(\mathbf{x})$ 或簡寫成 $\sigma^2_x$

標準差的特殊性質
---
對於常數$c$和隨機變數 $\mathbf{x}$ 和 $\mathbf{y}$:
$$
\begin{aligned}
\sigma(\mathbf{x}+c) &= \sigma(\mathbf{x})\\
\sigma(cx) &= c \sigma(\mathbf{x})\\
\sigma(\mathbf{x}+\mathbf{y}) &= \sqrt{\sigma^2(\mathbf{x})+\sigma^2(\mathbf{y})+\mathrm{cov}(\mathbf{x},\mathbf{y})}
\end{aligned}
$$
其中 $\mathrm{cov}(\mathbf{x},\mathbf{y})$ 是表示隨機變數 $\mathbf{x}$ 和 $\mathbf{y}$ 的共變異數。

若離散隨機變數 $\mathbf{y} = a\mathbf{x}+b$，其中 $a$ 和 $b$ 為常數。則:
$$
\begin{aligned}
\mu(\mathbf{y}) &= a \mu(\mathbf{x})+b\\
\sigma(\mathbf{y}) &= a \sigma(\mathbf{x})\\
\sigma^2(\mathbf{y}) &= a^2 \sigma^2(\mathbf{x})\\
\end{aligned}
$$

樣本推估母體的性質
---
在現實，一個母體的真正的標準差並不一定找的到。通常母體的性質是通過隨機抽取一定量的樣本來估計的。假設樣本資料 $x_1$、$x_2 \cdots x_n$ 是從一個平均數 $\mu$ 而變異數 $\sigma^2$ 之母體 $\mathbf{X}$ 中經由隨機抽樣而來。以概率來說
$$
\mu = E(\mathbf{X})
$$
$$
\sigma^2 = E(\mathbf{X}-E(\mathbf{X}))^2 = E(\mathbf{X}-\mu)^2
$$
其中$E$是期望值。此外
$$
\begin{aligned}
E(\mathbf{XY})&=E(\mathbf{X})E(\mathbf{Y})\\
\sigma(\mathbf{x})&=E(\mathbf{x}^2)-E(\mathbf{x})^2
\end{aligned}
$$
當隨機變數 $\mathbf{X}$ 和 $\mathbf{Y}$ 的共變異數為0，又稱它們不相關時，變異數也有可加性
$$
\sigma(\mathbf{X}+\mathbf{Y})=\sigma(\mathbf{X})+\sigma(\mathbf{Y})
$$
當$n$趨於無窮，則樣本平均會與母體平均趨於相同。
- 樣本平均
$$
\bar{x} = \frac{1}{n}\left.( x_1 + x_2 + \cdots + x_n\right.)=\frac{1}{n} \sum_{i=1}^n x_i
$$
proof:

證明樣本平均數 $\bar{x}$ 是母體平均數 $\mu$ 之不偏估計量
$$
\begin{aligned}
E(\bar{x}) &= E\left( \frac{x_1 + x_2 + \cdots + x_n}{n}  \right)\\
           &= \frac{1}{n}E\left( x_1 + x_2 + \cdots + x_n  \right)\\
           &= \frac{1}{n}\left( E(x_1) + E(x_2) + \cdots + E(x_n)  \right)\\
           &= \frac{1}{n}\left( \mu + \mu + \cdots + \mu  \right)\\
           &= \frac{1}{n}\left( n\mu \right)\\
           &= \mu\\
\end{aligned}
$$
- 樣本標準差
$$
s^2 = \frac{1}{n-1}\left.[ (x_1-\mu)^2+(x_2-\mu)^2+\cdots+ (x_n-\mu)^2\right.]
$$
proof:

證明樣本變異數 $s^2$ 是母體變異數 $\sigma^2$ 的不偏估計量
$$
\begin{aligned}
E(s^2) &= E\left( \frac{1}{n-1}\sum_{i=1}^n(x_i-\bar{x})^2  \right)\\
       &= E\left( \frac{1}{n-1}\sum_{i=1}^n(x_i^2-2x_i\bar{x}+\bar{x}^2)  \right)\\
       &= \frac{1}{n-1} E\left( \sum_{i=1}^n x_i^2-2\bar{x}\sum_{i=1}^n x_i+n\bar{x}^2  \right)\\
       &= \frac{1}{n-1} E\left( \sum_{i=1}^n x_i^2-2n\bar{x}^2+n\bar{x}^2  \right)\\
       &= \frac{1}{n-1} E\left( \sum_{i=1}^n x_i^2-n\bar{x}^2  \right)\\
       &= \frac{1}{n-1} \left( \sum_{i=1}^n E(x_i^2)-nE(\bar{x}^2)  \right)\\
       &= \frac{1}{n-1} \left[\sum_{i=1}^n \left(Var(x_i)+E(x_i)^2\right)-nE\left(Var(\bar{x})+E(\bar{x})^2\right) \right]\\
       &= \frac{1}{n-1} \left[n\left(\sigma^2+\mu^2\right)-n({\color{red}\sigma^2_{\bar{x}}}+\mu^2) \right]\\
       &= \frac{1}{n-1} \left[n\left(\sigma^2+\mu^2\right)-n({\color{red}\frac{\sigma^2}{n}}+\mu^2) \right]\\
       &= \frac{1}{n-1} \left(n\sigma^2+n\mu^2-\sigma^2-n\mu^2 \right)\\
       &= \frac{1}{n-1} \left(n-1\right)\sigma^2 \\
       &= \sigma^2
\end{aligned}
$$
- 標準誤差

抽取多份大小為 $n$ 的樣本，每個樣本各有一個平均值，當各樣本為獨立時，所有這個大小的樣本之平均值的標準差可證明為
$$
\sigma_{\bar{x}} = \frac{\sigma_x}{\sqrt{n}}
$$
proof:

$$
\begin{aligned}
\sigma_{\bar{x}}^2 &= Var(\bar{x})\\
                   &= Var(\frac{1}{n} \sum_{i=1}^n x_i)\\
                    &= \frac{1}{n^2}Var( \sum_{i=1}^n x_i)\\
                    &= \frac{n}{n^2}\sigma_x^2\\
                    &= \frac{1}{n}\sigma_x^2\\
\end{aligned}
$$ 