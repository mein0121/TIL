## 벡터화 - 벡터 연산
- 같은 형태(shape)의 배열(벡터, 행렬)간의 연산은 같은 index의 원소끼리 연산을 한다. 
    - **Element-wise(원소별) 연산** 이라고도 한다.
    - 배열간의 연산시 배열의 형태가 같아야 한다.
    - 배열의 형태가 다른 경우 Broadcast 조건을 만족하면 연산이 가능하다.
### 벡터/행렬과 스칼라간 연산

$$
\begin{align}
x=
\begin{bmatrix}
1 \\
2 \\
3 \\
\end{bmatrix}
\end{align}
$$

$$
\begin{align}
10 - x = 10 -
\begin{bmatrix}
1 \\
2 \\
3 \\
\end{bmatrix}
=
\begin{bmatrix}
10 - 1 \\
10 - 2 \\
10 - 3 \\
\end{bmatrix}
=
\begin{bmatrix}
9 \\
8 \\
7 \\
\end{bmatrix}
\end{align}
$$
$$
\begin{align}
10 \times
\begin{bmatrix}
1 & 2 \\
3 & 4
\end{bmatrix}
=
\begin{bmatrix}
10\times1 & 10\times2 \\
10\times3 & 10\times4 \\
\end{bmatrix}
=
\begin{bmatrix}
10 & 20 \\
30 & 40
\end{bmatrix}
\end{align}
$$
### 벡터/행렬의 연산
$$
\begin{align}
\begin{bmatrix}
1 \\
2 \\
3 \\
\end{bmatrix}
+
\begin{bmatrix}
10 \\
20 \\
30 \\
\end{bmatrix}
=
\begin{bmatrix}
1 + 10 \\
2 + 20 \\
3 + 30 \\
\end{bmatrix}
=
\begin{bmatrix}
11 \\
22 \\
33 \\
\end{bmatrix}
\end{align}
$$

$$
\begin{align}
\begin{bmatrix}
1 \\
2 \\
3 \\
\end{bmatrix}
-
\begin{bmatrix}
10 \\
20 \\
30 \\
\end{bmatrix}
=
\begin{bmatrix}
1 - 10 \\
2 - 20 \\
3 - 30 \\
\end{bmatrix}
=
\begin{bmatrix}
-9 \\
-18 \\
-27 \\
\end{bmatrix}
\end{align}
$$
$$
\begin{align}
\begin{bmatrix}
1 & 2 \\
3 & 4
\end{bmatrix}
+
\begin{bmatrix}
10 & 20 \\
30 & 40 \\
\end{bmatrix}
=
\begin{bmatrix}
1+10 & 2+20 \\
3+30 & 4+40
\end{bmatrix}
\end{align}
$$