# Base

## 메인 루프

엔진이 탑재되는 메인 루프 부분.

```cpp
while (true)
    {
        if (PeekMessage(&msg, nullptr, 0, 0, PM_REMOVE)) { //메세지 큐에서 순차적으로 꺼내 사용
            if (msg.message == WM_QUIT)
                break;

            if (!TranslateAccelerator(msg.hwnd, hAccelTable, &msg))
            {
                TranslateMessage(&msg);
                DispatchMessage(&msg);
            }
        }
        //TODO : 게임 로직
        game->Update();
    }

    return (int) msg.wParam;
```
기존 `GetMessage` 는 메세지 수신 전까지 루프가 정지하는 방식이기 때문에, 메시지 유무 확인만 하고 루프가 계속되는 `PeekMessage`를 사용.

## DLL vs Lib

라이브러리는 크게 정적 라이브러리와 동적 라이브러리가 있다.두 라이브러리의 가장 큰 차이점은 실행파일에 링킹되는 시점이다.
* 정적라이브러리 의 경우 **컴파일**시 실행 파일로 복사된다.
* 동적라이브러리 의 경우 obj 파일 생성시 복사되었던 모듈의 주소값을 **런타임**시 접근한다.

본 프로젝트에서는 lib 형태로 엔진을 생성한다.
