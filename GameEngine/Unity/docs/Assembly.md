# Assembly (어셈블리)

Unity에서 사용하는 어셈블리는 코드를 묶어서 컴파일하는 단위를 뜻한다.

저수준 언어를 뜻하는 어셈블리어와는 이름만 같고 다른 개념이다.

---

## 📂 네임스페이스 vs 어셈블리

* **네임스페이스**

  * 클래스명 중복 방지·논리적 분리 용도

* **어셈블리**

  * Unity에서는 `.asmdef`로 묶인 폴더 단위가 **컴파일 단위**
  * 물리적(폴더) 분리, 참조·재컴파일 경계가 생김

> 💡 편의를 위해 네임스페이스와 어셈블리 단위를 일치시키면 관리가 더 수월합니다.
> Unity의 `asmdef.Root Namespace` 옵션을 활용해 보세요.

---

## 🔗 참조 방향

* **단방향 참조**만 허용

  ```
  A → B → C   (O)
  A ↔ B      (X)
  ```

* **순환 참조가 필요할 땐**

  * C# **이벤트(Event)** 로 메시지 전달
  * 또는 **의존성 역전(DIP)** (인터페이스·DI) 활용

---

## 🛠 컴파일 단위 & 재컴파일 범위

1. 스크립트 수정 → **해당 어셈블리**만 다시 컴파일
2. **참조가 연결된 어셈블리**는

   * 수정한 어셈블리의 **공용 API**(public 멤버)가 바뀌었을 때만 함께 다시 컴파일

> 공용 API가 변경되면 결국 두 어셈블리의 코드를 직접 수정해줘야 하기 때문에, 수정된 어셈블리만 다시 컴파일되는 것이 맞다.

---

## ⏳ 컴파일 순서

1. **`.asmdef` 어셈블리들**
2. **`.asmref` 임시 어셈블리**
3. **Assembly-CSharp** (기본 스크립트)

```
asmdef → asmref → Assembly-CSharp
```

> 따라서 asmdef 어셈블리에서 Assembly-CSharp 어셈블리를 참조할 수 없다.

---

## 📝 어셈블리 선언·설정

1. **Assembly Definition Asset(`.asmdef`) 생성**

   * 해당 폴더와 하위 스크립트를 묶어 컴파일 단위로 지정

2. 주요 설정

   * **Name**: 어셈블리 식별명
   * **Auto Referenced**
     * `true`: 이 asmdef이 **Assembly-CSharp**에서 자동 참조 허용
     * `false`: 명시적 참조(`Assembly Definition References Asset (.asmref)`) 필요

   * **Assembly Definition References**
     * 순환 참조가 아닌 asmdef을 참조

   * **Root Namespace**
     * 새 스크립트에 자동으로 붙일 기본 네임스페이스

---

## 🔀 Assembly Definition Reference(`.asmref`)

* **목적**: Auto-Referenced=false인 asmdef을 **Assembly-CSharp**에서 쓰고 싶을 때
* **동작**:

  1. `.asmref`가 있는 폴더를 **임시 어셈블리**로 분리
  2. 위 임시 어셈블리가 Assembly-CSharp에서 참조 가능해짐
* **주의**: 이 임시 어셈블리에서 Assembly-CSharp 스크립트는 참조 불가능 해짐

```
[1] asmdef 어셈블리들  
[2] .asmref 폴더 (임시 어셈블리)  
[3] Assembly-CSharp 어셈블리
```

---

