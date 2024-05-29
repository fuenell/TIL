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

### 3. 게임정보(DB) 참조
테이블 명이 `USER_DATA`이면 아래와 같이 참조한다.
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

            int level = int.Parse(gameDataJson[0]["level"]);
            string profileMessage = gameDataJson[0]["profileMessage"].ToString();
        }
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
});
```

### 4. 차트 참조
원천 데이터 (고정된 데이터)

``` C#
Backend.Chart.GetChartContents("123456", callback =>    // 차트 ID가 123456 일 때
{
    if (callback.IsSuccess())
    {
        JsonData jsonData = callback.FlattenRows();

        if (0 < jsonData.Count)
        {
            for (int i = 0; i < jsonData.Count; i++)
            {
                int level = int.Parse(jsonData[i]["level"].ToString());
                int maxExperience = int.Parse(jsonData[i]["maxExperience"].ToString());
                int rewardGold = int.Parse(jsonData[i]["rewardGold"].ToString());

                Debug.Log($"{level} lv / {maxExperience} exp / {rewardGold} gold");
            }
        }
    }
});
```

