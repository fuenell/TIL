# 논리 (Logic)

참과 거짓을 구분한다



## 논리 연산자
1. 논리곱 (AND)
- `A • B`
A와 B 둘다 참이라면 참  

2. 논리합 (OR)
- `A + B`
A나 B 둘 중 하나라도 참이면 참  

3. 부정 (NOT)
- `~A`
A가 거짓이면 참  

4. 배타적 논리합 (XOR)
- `A ⊕ B`
A와 B가 다르면 참

## 동치법칙

1. 교환법칙
   `A • B ≡ B • A`  
   `A + B ≡ B + A`  

2. 결합법칙
   `(A • B) • C ≡ A • (B • C)`  
   `(A + B) + C ≡ A + (B + C)`  

3. 분배법칙
   `A + (B • C) ≡ (A + B) • (A + C)`  
   `A • (B + C) ≡ (A • B) + (A • C)`  

4. 항등법칙
   `A • TRUE ≡ A`  
   `A + FALSE ≡ A`  

5. 지배법칙
   `A • FALSE ≡ FALSE `  
   `A + TRUE ≡ TRUE `  

6. 부정법칙
   `~TRUE ≡ FALSE `  
   `~FALSE ≡ TRUE `  
   `A • (~A) ≡ FALSE`  
   `A + (~A) ≡ TRUE`  

7. 이중 부정법칙
   `~(~A) ≡ A`  
   
8. 멱등법칙
   `A • A ≡ A`  
   `A + A ≡ A`  

9. 드 모르간 법칙
   `~(A • B) ≡ (~A) + (~B)`  
   `~(A + B) ≡ (~A) • (~B)`  
   
10. 흡수법칙
      `A • (A + B) ≡ A`  
      `A + (A • B) ≡ A`  
