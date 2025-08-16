# TA-Lib源码阅读

#### TA_LINEARREG_ANGLE

```c
TA_LIB_API TA_RetCode TA_LINEARREG_ANGLE(int startIdx, int endIdx, const double inReal[], 
    int optInTimePeriod, int *outBegIdx, int *outNBElement, double outReal[])
{

    outIdx = 0; /* Index into the output. */
    today = startIdx;

    SumX = optInTimePeriod * ( optInTimePeriod - 1 ) * 0.5;
    SumXSqr = optInTimePeriod * ( optInTimePeriod - 1 ) * ( 2 * optInTimePeriod - 1 ) / 6;
    Divisor = SumX * SumX - optInTimePeriod * SumXSqr;

    while( today <= endIdx )
    {
        SumXY = 0;
        SumY = 0;
        for( i = optInTimePeriod; i-- != 0; )
        {
            SumY += tempValue1 = inReal[today - i];
            SumXY += (double)i * tempValue1;
        }
        m = ( optInTimePeriod * SumXY - SumX * SumY) / Divisor;
        outReal[outIdx++] = std_atan(m) * ( 180.0 / PI );
        today++;
    }

}
```

- 自然数的求和公式

$$
1 + 2 + \cdots + N = \frac{N(N+1)}{2}
$$

对应代码：

```c
// 这里计算0到n-1的和
SumX = optInTimePeriod * ( optInTimePeriod - 1 ) * 0.5;
```

- 自然数的平方和公式

$$
1^2 + 2^2 + \cdots + N^2 = \frac{N(N+1)(2N+1)}{6}
$$

对应代码：

```c
// 这里计算0到n-1的平方和
SumXSqr = optInTimePeriod * ( optInTimePeriod - 1 ) * ( 2 * optInTimePeriod - 1 ) / 6;
```

- 线性回归模型斜率和截距计算公式

对于线性回归：

$$
y=\alpha+\beta x
$$

计算斜率和截距：

$$
\begin{aligned}
 & \widehat{\alpha}=\bar{y}-(\widehat{\beta}\left.\bar{x}\right), \\
 & \widehat{\beta}=\frac{\sum_{i=1}^n(x_i-\bar{x})(y_i-\bar{y})}{\sum_{i=1}^n(x_i-\bar{x})^2}=\frac{\sum_{i=1}^n\Delta x_i\Delta y_i}{\sum_{i=1}^n\Delta x_i^2}
\end{aligned}
$$

扩展公式：

$$
\begin{aligned}
\widehat{\alpha} & =\frac{\sum_{i=1}^ny_i\sum_{i=1}^nx_i^2-\sum_{i=1}^nx_i\sum_{i=1}^nx_iy_i}{n\sum_{i=1}^nx_i^2-(\sum_{i=1}^nx_i)^2} \\
 \\
\widehat{\beta} & =\frac{n\sum_{i=1}^nx_iy_i-\sum_{i=1}^nx_i\sum_{i=1}^ny_i}{n\sum_{i=1}^nx_i^2-(\sum_{i=1}^nx_i)^2}
\end{aligned}
$$

对应代码：

```
Divisor = SumX * SumX - optInTimePeriod * SumXSqr;

for( i = optInTimePeriod; i-- != 0; )
{
    SumY += tempValue1 = inReal[today - i];
    SumXY += (double)i * tempValue1;
}
m = ( optInTimePeriod * SumXY - SumX * SumY) / Divisor;
```

斜率和夹角的关系：

$$
\hat{\beta}=\tan(\theta)=\frac{dy}{dx}
$$

对应代码：

```c
// std_atan(y/x)函数求y/x的反正切，返回单位为弧度，返回值范围为[-PI/2,PI/2]
outReal[outIdx++] = std_atan(m) * ( 180.0 / PI );
```

#### 参考资料

- [Simple linear regression](https://en.wikipedia.org/wiki/Simple_linear_regression)