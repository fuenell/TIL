# 뒤끝 (Backend)
게임 개발용 서버

https://docs.thebackend.io/

### 1. 시작하기
SDK 추가 후 토큰만 연결하면 됨

2. 문법
``` C#
// 로그인
Backend.BMember.CustomLogin(id, pw, callback =>
{
    if (callback.IsSuccess())
    {
        print(id + "님 환영합니다.");
    }
    else
    {
        print(callback.GetStatusCode());  // 401-계정 정보 틀림, 403-차단 유저, 410-탈퇴 중
    }
});

// 게임정보 참조

// 차트 참조
```
