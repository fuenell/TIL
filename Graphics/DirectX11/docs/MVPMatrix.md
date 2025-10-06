# MVP 행렬
MVP 행렬은 로컬 좌표에 위치한 버텍스를 카메라가 보는 좌표를 기준으로 변환해주는 행렬이다

MVP는 `Model`, `View`, `Projection` 행렬의 줄임말로 총 3개의 행렬을 합친 행렬을 뜻한다.

## 행우선, 열우선
벡터를 행으로 보느냐 열로 보느냐에 따라 행렬 곱셈 순서가 달라진다

`행우선(Row-Major)`은 벡터를 행으로 표현하고 `벡터 * 행렬` 순서로 계산

`(1x4 행렬) * (4x4 행렬) = (1x4 행렬)` → 결과도 행벡터

$$
\begin{bmatrix} x & y & z & 1 \end{bmatrix}
\begin{bmatrix}
m_{11} & m_{12} & m_{13} & m_{14} \\
m_{21} & m_{22} & m_{23} & m_{24} \\
m_{31} & m_{32} & m_{33} & m_{34} \\
m_{41} & m_{42} & m_{43} & m_{44}
\end{bmatrix}
= [x', y', z', w']
$$


`열우선(Column-Major)`은 벡터를 열로 표현하고 `행렬 * 벡터` 순서로 계산

`(4x4 행렬) * (4x1 행렬) = (4x1 행렬)` → 결과도 열벡터

$$
\begin{bmatrix}
m_{11} & m_{12} & m_{13} & m_{14} \\
m_{21} & m_{22} & m_{23} & m_{24} \\
m_{31} & m_{32} & m_{33} & m_{34} \\
m_{41} & m_{42} & m_{43} & m_{44}
\end{bmatrix}
\begin{bmatrix} x \\ y \\ z \\ 1 \end{bmatrix}
= \begin{bmatrix} x' \\ y' \\ z' \\ w' \end{bmatrix}
$$

DirectX 에서는 기본적으로 `행우선(Row-Major)` 방식을 사용하기 때문에 행우선 방식으로 설명

## Model 행렬
로컬 좌표의 버텍스를
월드 상에 모델이 위치한 변환하는 행렬

World 행렬이라고도 부르기 때문에 MVP를 WVP라고 부를 수도 있다

Model 행렬은 월드상의 위치, 회전, 크기를 변환한다

변환 순서가 중요한데

먼저 크기를 변환하고
그 다음 회전
그 다음 위치 이동을 해야 한다

왜냐면 크기와 회전 행렬은 원점을 기준으로 작성이 되어있기 때문이다.

열우선 방식이기 때문에
좌측에서 우측 방향으로 계산하기 때문에 아래와 같이 계산하면 된다

```
M = 크기 행렬 * 회전 행렬 * 이동 행렬
M = S(스케일) * R(회전) * T(이동)
M = SRT
```

### 크기 행렬
각 버텍스의 요소에 크기만큼 곱하면?
크기가 커진다
0,1,2 -> x2 -> 0,2,4

따라서 크기 행렬은 다음과 같다

$$
\begin{bmatrix}
S{x} & 0 & 0 \\
0 & S{y} & 0 \\
0 & 0 & S{z} \\
\end{bmatrix}
$$

### 회전 행렬
2차원 기준 회전 행렬은 원점을 기준으로 θ 만큼 회전하는 행렬은 아래와 같다

$$
\begin{bmatrix}
\cos(\theta) & -\sin(\theta) \\
\sin(\theta) & \cos(\theta)
\end{bmatrix}
$$

왜냐하면 1,0 / 0,1 에서 θ만큼 회전하면 다음과 같이 변하기 때문에

<img width="899" height="649" alt="Image" src="https://github.com/user-attachments/assets/c53091ea-bc58-43b5-b530-c28c086901ca" />

3차원은 각 축별 회전 표현이 다음과 같다


#### **X축 기준 회전 (Roll)**
* X좌표는 고정되고, YZ 평면에서 2D 회전이 일어납니다.

$$
R_x(\theta) =
\begin{bmatrix}
1 & 0 & 0 \\
0 & \cos(\theta) & -\sin(\theta) \\
0 & \sin(\theta) & \cos(\theta)
\end{bmatrix}
$$

#### **Y축 기준 회전 (Pitch)**
* Y좌표는 고정되고, XZ 평면에서 2D 회전이 일어납니다. (부호에 주의)

$$
R_y(\theta) =
\begin{bmatrix}
\cos(\theta) & 0 & \sin(\theta) \\
0 & 1 & 0 \\
-\sin(\theta) & 0 & \cos(\theta)
\end{bmatrix}
$$

#### **Z축 기준 회전 (Yaw)**
* Z좌표는 고정되고, XY 평면에서 2D 회전이 일어납니다.

$$
R_z(\theta) =
\begin{bmatrix}
\cos(\theta) & -\sin(\theta) & 0 \\
\sin(\theta) & \cos(\theta) & 0 \\
0 & 0 & 1
\end{bmatrix}
$$

X축, Y축, Z축 회전 순으로 행렬을 곱하게 되면 XYZ 회전이 된다

각 축의 회전을 순서대로 곱하기 때문에 오일러락이 발생하게 된다
(회전한 상태의 축에서 또 회전을 하기 때문에)

### 이동 행렬

이미 크기 행렬과 회전 행렬이 곱해진 행렬에서 순수하게 이동만 추가하기 힘들다

따라서 임시로 행렬을 확장해서 확장된 열에 이동할 값을 넣는다.
이를 아핀공간 이라고 한다

$$
\begin{bmatrix} x & y & z & 1 \end{bmatrix}
\begin{bmatrix}
1 & 0 & 0 & 0 \\
0 & 1 & 0 & 0 \\
0 & 0 & 1 & 0 \\
{a} & {b} & {c} & 1
\end{bmatrix}
= [x+{a}, y+{b}, z+{c}, 1]
$$

최종적으로 크기, 회전, 이동 행렬을 모두 곱해서 Model 행렬을 만들 수 있다.

## View 행렬
View 행렬은 모델을 카메라 앞으로 정렬 시키는 행렬이다.

카메라의 위치의 역행렬을 구하면 된다?


## Projection 행렬

잘 구하면 됨