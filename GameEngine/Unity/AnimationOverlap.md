# 애니메이션 오브젝트 On/Off 시 애니메이션 Key 값 겹침 현상 해결
## 발생 버전:
`~2022.1` (이후 버전에도 따로 개선해주지 않을 것으로 보임)
## 문제: 
애니메이션이 실행 중인 오브젝트를 껐다가 켰을 때, 오브젝트의 디폴트 값들이 마지막 애니메이션 키들로 고정되는 문제

![animation_overlap](https://user-images.githubusercontent.com/37904040/170420008-5f0865a9-b57f-430c-8673-b08a2f4ba294.gif)

---

## 해결책 1:
애니메이션이 실행 중인 오브젝트를 끄기 직전에 `Animator.Rebind()`를 호출한다.  
`Rebind()`는 애니메이터의 State와 Parameters를 초기화 시키는 함수로 애니메이터을 초기상태로 되돌린다.
``` C#
void HideThisObject()
{
    this.GetComponent<Animator>().Rebind();
    this.gameObject.SetActive(false);
}
```

#### 장점:
- 코드 한 줄을 추가해서 대부분의 문제를 해결할 수 있다.
- Trigger 같은 파라미터는 오브젝트가 꺼질 때 초기화가 필요한 경우가 있는데 Rebind 호출 시 자동으로 초기화해준다.
#### 단점:
- 해당 오브젝트를 끄는 코드를 모두 수정해 줘야 한다.
- 파라미터 값을 유지해야 할 경우에는 사용하기 힘들다.

---

## 해결책 2:
같은 Animator에서 사용되는 모든 애니메이션의 키를 동기화한다. 

ex) 애니메이션이 2개일 경우  
- 1번 애니메이션에서 사용되는 모든 키 복사 후 2번 애니메이션에 붙여넣기
- 2번 애니메이션에서 사용되는 모든 키 복사 후 1번 애니메이션에 붙여넣기

#### 장점:
- 이 방법의 경우에는 키 동기화 작업을 1회만 해준다면 스크립트 없이 문제를 해결할 수 있다.
#### 단점:
- 그러나 애니메이션의 개수가 많으면 키를 동기화하는 데 오래 걸릴 수 있다.  
- 또한 애니메이션이 추가된다면 키를 다시 동기화해줘야 하는 번거로움이 있다.

---

## 해결책 3:
Disable 애니메이션을 만들어서 오브젝트를 꺼야 할 때 끄지 않고 Disable 애니메이션을 재생시킨다. 

#### 장점:
- 애니메이션 개수와 상관없이 작업량이 고정적이고 한번 작업하면 추가적인 작업이 필요 없다.
- 오브젝트가 사라지는 애니메이션을 재생할 때 편하다.
#### 단점:
- 해당 오브젝트를 영구적으로 끌 수 없다.
- 해당 오브젝트를 끄는 코드를 모두 수정해 줘야 한다.

---

## 해결책 4:
`KeepAnimatorControllerStateOnDisable` 옵션을 사용해서 오브젝트를 껐다 켰을 때 애니메이션이 이어서 재생되도록 한다.  
그러면 해당 현상이 발생하지 않는다.
``` C#
void Awake()
{
    this.GetComponent<Animator>().keepAnimatorControllerStateOnDisable = true;
}
```

위와 같이 스크립트를 작성하거나 아래와 같이 Debug 모드에서 옵션을 선택해서 옵션을 활성화할 수 있다.

![image](https://user-images.githubusercontent.com/37904040/170427978-984dfeb7-d9de-4396-b722-ce63beed882b.png)

#### 장점:
- 작업량이 가장 적다.
#### 단점:
- 오브젝트를 껐다 켜도 이전 애니메이션이 이어서 재생 중이기 때문에, 애니메이션이 처음부터 시작하기를 원한다면 `OnEnable` 함수에서 첫 애니메이션을 재생시켜주어야 한다.
``` C#
void OnEnable()
{
    // this.GetComponent<Animator>().GetCurrentAnimatorStateInfo(0).fullPathHash; 으로 첫 애니메이션을 가져올 수도 있다.
    this.GetComponent<Animator>().Play(/*FirstAnimationName*/, 0, 0);
}
```
