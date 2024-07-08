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

### 5. 뒤끝 펑션
제공되는 함수 이외의 커스텀 함수를 서버에 등록하고 싶을 때 추가할 수 있다.
1. https://docs.thebackend.io/sdk-docs/function/work-in-windows/install 개발툴 + 템플릿 설치
2. 프로젝트에서 커스텀 함수 코드 작성
3. `debugConfig.json` 에 정보를 입력해 테스트를 진행
4. cmd에서 `backend config`를 입력해 `authKey` 에 펑션 키 추가
5. cmd에서 `backend build [프로젝트 경로]` 로 프로젝트를 `publish.zip` 으로 빌드 (`bin/Release/net6.0\linux-x64` 경로에 생성됨)
6. cmd에서 `backend deploy [함수명] [publish.zip 경로]` 로 프로젝트 서버로 배포

``` C#
public Stream Function(Stream stream, ILambdaContext context)
{
    try
    {
        // 뒤끝펑션 API 초기화
        Backend.Initialize(ref stream);
    }
    catch(Exception e)
    {
        // 뒤끝펑션 API 초기화를 실패한 경우
        return ReturnErrorObject("initialize " + e.ToString());
    }

    // debugConfig.json에서 `content` 혹은 뒤끝 SDK에서 InvokeFunction을 호출할 때 인자 값으로 넘긴 `Param` 에
    // value 키가 존재하는지 확인합니다.  
    if(Backend.HasKey("value") == false)
    {
      return ReturnErrorObject("value key is not exist");
    }

    // debugConfig.json에서 `content` 혹은 뒤끝 SDK에서 InvokeFunction을 호출할 때 인자 값으로 넘긴 `Param` 은
    // Backend.Content를 이용하여 엑세스할 수 있습니다.  
    return Backend.StringToStream(Backend.Content["value"].ToString());
}
```
