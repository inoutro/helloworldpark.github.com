---
layout: post
title:  "가챠의 폭력성 (1) - 이론편"
date:   2017-03-18 18:24:27 +0900
categories: Mathematics
---

1. [가챠의 폭력성 (1) - 이론편](https://helloworldpark.github.io/mathematics/2017/03/18/NoMoreGatcha001.html)
2. [가챠의 폭력성 (2) - 구현편](https://helloworldpark.github.io/mathematics/2017/03/18/NoMoreGatcha002.html)
3. [가챠의 폭력성 (3) - 분석편](https://helloworldpark.github.io/mathematics/2017/03/18/NoMoreGatcha003.html)

## 문제의 계기

이 포스트는 게임회사에 다니는 제 친구의 질문에서 시작되었습니다.

> 8개의 아이템을 다 모아야 하는데, 이 아이템은 뽑기 상자(즉, 가챠)를 하나 굴리면 하나가 나온다고 해보자.
> 이걸 다 모으려면 가챠를 평균 몇 번 돌려야 할까?

## 문제의 정의

이 문제를 조금 더 정밀하게 정의해보겠습니다.

> $$n\geq1$$개의 아이템으로 이루어진 집합 $$S_{n} = \{x_{1}, x_{2}, \cdots , x_{n}\}$$이 있다고 하자.
> 가챠를 한 번 돌리면 $$$S_{n}$$의 원소 중 하나가 나오는데, $$x_{i}$$가 나올 확률을 $$p_{i}$$라고 하자.
> 그렇다면 가챠를 평균 몇 번 돌리면 집합 $$$S_{n}$$의 모든 원소를 다 모을 수 있을까?

일단 임의의 확률분포에 대해 생각하는 것은 너무 머리가 아프니, 일단은 각각의 아이템이 나올 확률이 모두 같다고 가정하겠습니다.

> 단, $$p_{i} = \frac{1}{n}$$으로 가정한다.

## 확률 구하기

먼저, $$m$$번째 뽑기에서 아이템을 다 모았다고 가정하겠습니다(당연히 $$m \geq n$$입니다). 그렇다면 $$m$$번째까지 뽑기를 하는 동안 아이템을 뽑을 경우의 수는 $$n^{m}$$입니다.

그리고 $$m$$번째에 $$x_{1}$$을 뽑음으로써 아이템을 다 모은다고 가정하겠습니다. 그렇다면 그 직전의 $$(m-1)$$번의 뽑기 동안 $$x_{1}$$은 절대 나오면 안 되고, $$x_{2},x_{3},\cdots,x_{n}$$이 전부 한 번 이상은 반드시 나와야 합니다(만약 그렇지 않다면, $$x_{1}$$을 뽑음으로써 아이템을 다 모은다는 가정에 어긋납니다).

이러한 경우의 수를 세는 것은 $$X_{m-1} = \{1,2,\cdots,m-1\}$$에서 $$Y_{n-1} = \{1,2,\cdots,n-1\}$$으로 가는 전사 함수(Surjective function, 함수를 **화살 쏘기**라고 하면 $$Y$$의 원소들 중에서 $$X$$로부터 화살을 맞지 않은 녀석은 없는 경우가 없어야 한다는 뜻입니다)의 경우의 수를 세는 것과 똑같습니다. 이 경우의 수를 $$F(m, n)$$이라고 한다면(실제로 필요한 건 $$F(m-1,n-1)$$이지요), 이 숫자는 **포함과 배제**의 원리를 이용하여 구할 수 있습니다.

\begin{align}
F(m,n) &= \binom{n}{0} n^{m} - \binom{n}{1} (n-1)^{m} + \cdots \pm \binom{n}{n-1} 1^{m} \\\
       &= \sum_{k=1}^{n} (-1)^{k-1} \binom{n}{k - 1} (n - k + 1)^{m}
\end{align}

그런데 마지막으로 뽑는 아이템이 반드시 $$x_{1}$$일 필요는 없습니다. 마지막으로 뽑을 아이템은 $$n$$개 중 어느 것이라도 될 수 있으므로, 우리가 구해야 하는 경우의 수는 $$n F(m-1,n-1)$$입니다. 그러므로, $$m$$번째 뽑기에서 $$n$$개의 아이템을 다 모을 확률은 다음과 같습니다.

\begin{align}
p_{n}(m) = n \sum_{k=1}^{n-1} (-1)^{k-1} \binom{n - 1}{k - 1} (1-\frac{k}{n})^{m-1}
\end{align}

## 기대값 구하기

우리의 문제에서 기대값은 아래와 같이 정의됩니다.

\begin{align}
M_{n} = \sum_{m=n}^{\infty} m p_{n}(m)
\end{align}

식을 좀 더 전개하면 다음과 같습니다.

\begin{align}
M_{n} &= \sum_{m=n}^{\infty} m p_{n}(m) \\\
      &= \sum_{m=n}^{\infty} m n \sum_{k=1}^{n-1} (-1)^{k-1} \binom{n - 1}{k - 1} (1-\frac{k}{n})^{m-1} \\\
      &= n \sum_{m=n}^{\infty} \sum_{k=1}^{n-1} m (-1)^{k-1} \binom{n - 1}{k - 1} (1-\frac{k}{n})^{m-1} \\\
      &= n \sum_{k=1}^{n-1} \sum_{m=n}^{\infty} m (-1)^{k-1} \binom{n - 1}{k - 1} (1-\frac{k}{n})^{m-1} \\\
      &= n \sum_{k=1}^{n-1} (-1)^{k-1} \binom{n - 1}{k - 1} \sum_{m=n}^{\infty} m (1-\frac{k}{n})^{m-1} \\\
\end{align}

여기에서 시그마의 순서를 한 번 바꾸었는데, 사실 막 바꾸면 안 되긴 합니다. 무한히 더하기 때문에 합의 순서를 바꾸기 전에 한 번 따지기는 해야 하는데, 이 경우에는 합치는 함수의 합이 수렴하기 때문에 **바꿔도 됩니다**. 더 정확한 것은 좀 더 아래에서 따져보도록 하겠습니다.

결국 남은 것은 $$\sum_{m=n}^{\infty} m (1-\frac{k}{n})^{m-1}$$의 합을 구하는 것입니다. 그런데 이 값을 구하는 것은 결국은 $$\sum_{m=n}^{\infty} m r^{m-1}$$을 구하는 것과 같습니다.

\begin{align}
S(r) &= \sum_{m=n}^{\infty} r^{m} = \frac{r^{n}}{1-r} \\\
\frac{\partial}{\partial r} S(r) &= \sum_{m=n}^{\infty} m r^{m-1} = \frac{\partial}{\partial r} \left( \frac{r^{n}}{1-r} \right) \\\
&= \frac{r^{n-1}}{1-r} (n - n r + r) \\\
\end{align}

이 식을 대입하고 정리하면 다음을 얻습니다.

\\\[ M_{n} = \sum_{k=0}^{n-1} (-1)^{k} \binom{n}{k+1} \left(n-1 + \frac{n}{k+1} \right) \left(1 - \frac{k+1}{n} \right)^{n-1} \\\]

이 공식을 이용하여 1부터 10까지의 기대값을 컴퓨터로 계산하면 아래의 결과를 얻습니다.

\begin{array}{c|c}
n & M_{n} \\\
\hline
1 & 1 \\\
2 & 3 \\\
3 & 5.5 \\\
4 & 8.333 \\\
5 & 11.416 \\\
6 & 14.7 \\\
7 & 18.15 \\\
8 & 21.743 \\\
9 & 25.461 \\\
10 & 29.290 \\\
\end{array}

## 그다지 쓸데없는 수학적 관찰들

### 확률을 구하는 다른 방법

우리는 위에서 $$F(m,n)$$을 포함과 배제의 원리를 이용해 직접 구했지만, 사실 이 문제는 더 일반화하면 크기가 $$m$$인 집합을 공집합이 없는 $$n$$개의 부분집합으로 분할하는 경우의 수를 안다면 더 간단하게 표현할 수 있습니다. 이를 구하는 경우의 수의 공식이 [제 2종 스털링 수](https://en.wikipedia.org/wiki/Stirling_numbers_of_the_second_kind)입니다. 제 2종 스털링 수를 $$S(m,n)$$으로 표현한다면, $$F(m,n)$$은 다음과 같습니다.

\\\[ F(m,n) = n! S(m, n)\\\]

### 제 2종 스털링 수의 상한
제2종 스털링 수 $$S(m, n)$$은 아무리 커도 다음 수를 넘지 못 합니다.

\\\[ S(m, n) \leq \frac{1}{2}\binom{m}{n}n^{m-n} \\\]

### 확률의 합이 1임을 확인하기

이제 확률은 다음과 같이 표현됩니다.

\begin{align}
p_{n}(m) = \left( \frac{1}{n} \right)^{m} n! S(m-1, n-1)
\end{align}

그런데 제 2종 스털링 수는 다음의 생성함수를 갖습니다.

\begin{align}
\sum_{m=0}^{\infty} S(m, n) x^{m+1} = \frac{1}{(n+1)!\binom{\frac{1}{x}}{n+1}}
\end{align}

그러므로 확률의 합은 다음과 같습니다.

\begin{align}
\sum_{m=1}^{\infty} p_{n}(m) &= \sum_{m=1}^{\infty} \left( \frac{1}{n} \right)^{m} n! S(m-1, n-1) \\\
                             &= n! \sum_{m=1}^{\infty} \left( \frac{1}{n} \right)^{m} S(m-1, n-1) \\\
                             &= n! \sum_{m=0}^{\infty} \left( \frac{1}{n} \right)^{m+1} S(m, n-1) \\\
                             &= n! \left( \frac{1}{n!\binom{\frac{1}{x}}{n} }\right)_{x=\frac{1}{n}} \\\
                             &= 1
\end{align}

저렇게 값을 넣을 수 있는 것은 다음의 이유 때문입니다.
\begin{align}
\left( \frac{1}{n} \right)^{m} n! S(m-1, n-1) &\leq \left( \frac{1}{n} \right)^{m} n! \frac{1}{2} \binom{m-1}{n-1} (n-1)^{m - n + 1} \\\
&= n! n^{-n+1} \frac{1}{2} \binom{m-1}{n-1} \left( \frac{n-1}{n} \right)^{m}
\end{align}

여기에서 $$n$$은 고정이고, $$\binom{m-1}{n-1}$$은 $$m$$에 대한 다항식이므로 수렴의 여부는 $$\left( \frac{n-1}{n} \right)^{m}$$에 의해 결정되는데 얘가 수렴하는 등비수열이므로, $$\sum_{m=1}^{\infty} p_{n}(m)$$은 수렴합니다. 그러므로 생성함수에 값을 넣는 것에 문제가 없습니다.

### 기대값의 수렴성에 대한 증명

기대값의 수렴성 역시 제 2종 스털링 수의 상한을 이용하면 증명할 수 있습니다.

### 조화급수와의 관계

~~이 부분은 제가 아직 증명하지는 못 했지만, 엑셀로 계산되어 나온 결과를 분석하다가 알게 된 것입니다.~~
이 문제는 [Coupon Collector's Problem](https://en.wikipedia.org/wiki/Coupon_collector%27s_problem)이라는 문제로, 그 평균이 알려져 있습니다.

\begin{align}
M_{n} &= \sum_{k=0}^{n-1} (-1)^{k} \binom{n}{k+1} \left(n-1 + \frac{n}{k+1} \right) \left(1 - \frac{k+1}{n} \right)^{n-1}  \\\
      &= n \sum_{k=1}^{n} \frac{1}{k}
\end{align}

\begin{array}{c|c|c|c}
n & M_{n} & M_{n} - M_{n-1} & M_{n} - 2 M_{n-1} + M_{n-2}  \\\
\hline
1 & 1       & \,    & \,  \\\
2 & 3       & 2     & \,  \\\
3 & 5.5     & 2.5   & 0.5 \\\
4 & 8.333   & 2.833 & 0.333 \\\
5 & 11.417  & 3.083 & 0.25 \\\
6 & 14.7    & 3.283 & 0.2 \\\
7 & 18.15   & 3.45  & 0.167 \\\
8 & 21.743  & 3.593 & 0.143 \\\
9 & 25.461  & 3.718 & 0.125 \\\
10 & 29.290 & 3.829 & 0.111 \\\
\end{array}

### 참고문헌

Wikipedia, [Stirling Number of Second Kind](https://en.wikipedia.org/wiki/Stirling_numbers_of_the_second_kind)

Wikipedia, [Coupon collector's problem](https://en.wikipedia.org/wiki/Coupon_collector%27s_problem)

Stack Exchange, [Probability distribution of the coupon collectors problem](http://math.stackexchange.com/questions/379525/probability-distribution-in-the-coupon-collectors-problem)
