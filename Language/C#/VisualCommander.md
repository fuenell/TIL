# Visual Commander
Visual Studio에서 매크로를 사용할 수 있게 해주는 확장 프로그램

## 설치 방법
1. 확장 > 확장 관리 > `VisualCommander` 검색 및 설치 > Visual Studio를 종료하여 설치 마무리

![image](https://github.com/user-attachments/assets/cecbaff4-2047-4e99-8038-c016eb8a07c1)

2. 확장 > VCmd > Commands... > Add

![image](https://github.com/user-attachments/assets/96eed933-882d-4b91-b93b-46516a7e8c13)

3. 커맨드 이름을 설정하고 코드 Language를 C#4.0을 선택


![image](https://github.com/user-attachments/assets/9a41a6d1-a22f-462d-b208-911d71ebe91c)

4. 커맨드 코드 작성 창에 아래 코드 붙여넣고 Save 버튼 클릭 후 창 닫기
``` C#
using EnvDTE;
using EnvDTE80;
using System.Threading.Tasks;
using System.Runtime.InteropServices;

public class C : VisualCommanderExt.ICommand
{
    public void Run(EnvDTE80.DTE2 DTE, Microsoft.VisualStudio.Shell.Package package) 
    {
        try
        {
            // 사용하지 않는 using 문 제거
            DTE.ExecuteCommand("Edit.RemoveAndSort");

            // 포맷 문서 (코드 정렬)
            DTE.ExecuteCommand("Edit.FormatDocument");

            // using 문 제거 또는 정렬이 끝날 때 까지, 0.1초 대기 후 실행
            Task.Run(async () =>
            {
                // 0.1초 대기 (비동기적으로 대기)
                await Task.Delay(100);

                DTE.ExecuteCommand("File.SaveAll");
            });
        }
        catch
        {
            // using 문 제거 또는 정렬이 실패했을 때 그냥 저장만 실행
            DTE.ExecuteCommand("File.SaveAll");
        }
    }
}
```

5. 도구 > 옵션 > 환경 > 키보드 > `vcmd` 검색 > `VCmd.Command01` 선택 > `바로 가기 키 누르기` 박스를 선택 후 `Ctrl+S` 입력 > 할당 버튼 클릭 > 확인

![image](https://github.com/user-attachments/assets/7b9f2f89-0e86-4db0-a6e6-c99f2ccc9eee)

## 사용법
평소처럼 저장할 일이 있을 때 `Ctrl+S`를 눌러서 코드 자동 정리 및 저장