# 벡터 (Vector)
벡터는 n개의 숫자 모음이다.

좌표와 같은 형식으로 표현한다.

하지만 그 값은 방향과 크기를 표현한다.

# 내적 (dot product)
내적은 두 벡터가 얼마나 겹치는지를 나타내는 값이다.

구하는 방법은 각 항끼리 곱한 값을 모두 더한 값이다.


### 내적 공식
$$\mathbf{a} \cdot \mathbf{b} = |\mathbf{a}| |\mathbf{b}| \cos\theta$$

제2 코사인 전개식으로 (피타고라스 확장) 

```
(한 변의 길이)² = (다른 두 변의 길이)²의 합 - 2 × (다른 두 변의 곱) × (두 변 사이 끼인각의 코사인 값)
직각 삼각형이 아닌 삼각형의 변의 길이 구하는법
한 점에서 수선의 발을 내려서 가상을 직각 삼각형을 만들어서 구하는 원리
```

<img width="672" height="469" alt="image" src="https://github.com/user-attachments/assets/fe279d42-5c46-4ea2-9972-a83e13854eda" />


### 요소로 쉽게 계산
$$\mathbf{a} \cdot \mathbf{b} = a_x b_x + a_y b_y + a_z b_z$$

이 값으로 두 벡터 사이의 각을 쉽게 구할 수 있다.

```
두 벡터의 각도가 90도 이하면 양수

두 벡터의 각도가 90도면 0

두 벡터의 각도가 90도 이상면 음수
```
$\cos\theta$ 의 특징이 그대로 있기 때문에

# 외적 (cross product)
외적은 3차원 벡터끼리만 가능한 연산으로

두 벡터에 모두 수직한 벡터를 구할 수 있다.

* **좌표 형식** 

$$\mathbf{a} \times \mathbf{b} = (a_y b_z - a_z b_y, a_z b_x - a_x b_z, a_x b_y - a_y b_x)$$

---

$$\mathbf{a} \times \mathbf{b} = \begin{vmatrix} \mathbf{i} & \mathbf{j} & \mathbf{k} \\ a_x & a_y & a_z \\ b_x & b_y & b_z \end{vmatrix} $$

여인수 전개

---

$$\mathbf{a} \times \mathbf{b} = (a_y b_z - a_z b_y)\mathbf{i} - (a_x b_z - a_z b_x)\mathbf{j} + (a_x b_y - a_y b_x)\mathbf{k}$$

---

$$\mathbf{a} \times \mathbf{b} = (a_y b_z - a_z b_y)\mathbf{i} + (a_z b_x - a_x b_z)\mathbf{j} + (a_x b_y - a_y b_x)\mathbf{k}$$

---


# 반사 벡터

