# Test Framework [#Manual](https://docs.unity3d.com/Packages/com.unity.test-framework@2.0/manual/index.html)
Test Framework는 테스트를 자동으로 진행하는 기능이다.

## 사용법
1. Packages Manager에서 `com.unity.test-framework`를 설치한다.
2. `Window > General > Test Runner`로 테스트 창을 연다.
3. 상단에 `PlayMode`를 선택한다.
4. Tests 스크팁트 폴더를 생성할 위치 선택 후 `Create PlayMode Test Assembly Folder`를 클릭한다.
5. `Create Test Script in current folder`를 클릭해 Tests 폴더에 테스트 스크립트를 생성한다.
6. 테스트 스크립트를 작성한다.
7. Test Runner창에서 테스트를 실행해 결과를 확인한다.

![image](https://github.com/normal111/TIL/assets/37904040/046d979a-386b-4fc6-b645-8b80c6e9fd40)

## Assembly Definition 추가
테스트 스크립트는 따로 Tests 어셈블리로 묶여 기존 스크립트를 참조할 수 없다.  
때문에 테스트할 스크립트를 새로운 어셈블리로 묶어 Tests 어셈블리에 추가해주어야 한다.  
방법은 아래와 같다.

1. 테스트할 스크립트가 있는 폴더에서 `우클릭 > Create > Assembly Definition` 로 어셈블리 파일을 생성한다.
2. 참조 불가능 오류 발생 시 생성한 어셈블리의 Assembly Definition References에 필요한 외부 어셈블리를 추가하고 Apply를 클릭한다. (PostProcessing, Cinemachine 등)
3. Tests 어셈블리의 Assembly Definition References에 생성한 어셈블리를 추가하고 Apply를 클린한다.

![image](https://github.com/normal111/TIL/assets/37904040/60371a84-5358-4586-873c-eb7464f1c203)


## Test Script
테스트 스크립트는 크게 `[SetUp]`, `[Test]`, `[UnityTest]` 로 나뉜다.  
`[SetUp]`은 테스트가 진행되기 전 미리 해주어야 하는 작업을 작성하고,  
`[Test]`는 런타임 시 테스트를 진행할 작업을 작성하고,  
`[UnityTest]`는 런타임 시 Coroutine에서 테스트할 작업을 작성한다.  
각 메소트에서 `Assert`로 테스트 성공 여부를 체크한다.

``` C#
[SetUp]
public void SetUp()
{
    GameObject gameObject = GameObject.Instantiate(Resources.Load<GameObject>("Prefabs/LoginManager"));
    _loginManager = gameObject.GetComponent<LoginManger>();
}

[Test]
public void NewTestScriptSimplePasses()
{
    result = _loginManager.Login();
    Assert.AreEqual(result, 1);    // result가 1이면 테스트 성공
}

[UnityTest]
public IEnumerator NewTestScriptWithEnumeratorPasses()
{
    result = _loginManager.Login();
    yield return null;
    Assert.IsTrue(result == 1);    // result가 1이면 테스트 성공
}
```
