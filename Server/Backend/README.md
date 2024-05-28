# 뒤끝 (Backend)
게임 개발용 서버

https://docs.thebackend.io/

### 1. 시작하기
SDK 추가 후 토큰만 연결하면 됨

### 2. 로그인
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

```

### 3. 게임정보 참조
``` C#
// 삽입
Param param = new Param()
{
    {"level", 1 },
    {"profileMessage", "test" },
};
Backend.GameData.Insert("USER_DATA", param, callback =>
{
    if (callback.IsSuccess())
    {
        gameDataRowInDate = callback.GetInDate();

        Debug.Log($"데이터 삽입 성공: {callback}");
    }
    else
    {
        Debug.Log($"데이터 삽입 실패: {callback}");
    }
});


// 조회
Backend.GameData.GetMyData("USER_DATA", new Where(), callback =>
{
    if (callback.IsSuccess())
    {
        JsonData gameDataJson = callback.FlattenRows();

        if (0 < gameDataJson.Count)
        {
            gameDataRowInDate = gameDataJson[0]["inDate"].ToString();

            level = int.Parse(gameDataJson[0]["level"]);
            profileMessage = gameDataJson[0]["profileMessage"].ToString();
        }
        else
        {
            Debug.Log("저장된 데이터가 없습니다");
        }
    }
    else
    {
        Debug.Log("저장된 데이터가 없습니다 " + callback);
    }
});

// 수정
Param param = new Param()
{
    {"level", 2 },
    {"profileMessage", "msg" },
};
Backend.GameData.UpdateV2("USER_DATA", gameDataRowInDate, Backend.UserInDate, param, callback =>
{
    if (callback.IsSuccess())
    {
        Debug.Log($"데이터 수정 성공: {callback}");
    }
    else
    {
        Debug.Log($"데이터 수정 실패: {callback}");
    }
});
```

### 4. 차트 참조

