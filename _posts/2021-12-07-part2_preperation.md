---
title: "Ch 1. C# 실습 준비"
excerpt: "Console 창에 텍스트 컬러를 설정하고 프레임을 관리하는 방법을 알아보자"

categories:
  - Algorithm
tags:
  - [CSharp, Algorithm, Data Structure, Rookiss]

toc: true
toc_sticky: true

date: 2021-12-07
last_modified_at: 2021-12-07
---

## Console

### Console.WriteLine

콘솔 창에 문자열 출력하고 자동으로 한 줄 띄워준다. (Line)

```csharp
Console.WriteLine("Hello")
Console.WriteLine();

// Hello
// (빈줄)
```

### Console.SetCursorPosition

콘솔 창의 커서 위치를 세팅한다.

- Console.SetCursorPosition(0, 0);
  - 커서가 콘솔 창의 맨 왼쪽 상단에 위치한다

### Console.CursorVisible

- 콘솔 창에서 커서 위치를 보이게 할건지 아닌지 결정
- Console.CursorVisible = false (커서 위치 안보이게 돼서 커서 깜빡이는게 사라짐)
- 콘솔 창에서 커서 위치가 안보이게 됨. 커서 깜빡 깜빡 하는거 없어짐.

### Console.ForegroundColor

- 콘솔 창에 있는 텍스트 등등의 컬러를 설정
- Console.ForegroundColor = ConsoleColor.Green; (텍스트를 초록색으로 변경)

## 프레임

프레임은 1초에 화면에 표시되는 횟수를 의미한다. 프레임이 60fps라면 1초에 60번 실행된다. 프레임이 높을수록, 즉 렌더링이 여러번 될수록 화면이 부드러워진다.

```csharp
using System;

namespace ConsoleApp1
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.CursorVisible = false;

            int lastTick = 0;

            while (true)
            {
                #region 프레임 관리
                int currentTick = System.Environment.TickCount;  // 밀리 세컨즈로 나타낸 현재시간.  1초 = 1000 밀리세컨즈
                int elapsedTick = currentTick - lastTick; // 경과 시간

                // 만약 경과한 시간이 1/30초보다 작다면
                if (elapsedTick < 1000 / 30)
                    continue;
                lastTick = currentTick;  // 마지막 측정 시간 업뎃
                #endregion

                // 1.입력

                // 2.로직

                // 3.렌더링
                Console.SetCursorPosition(0, 0);
                Console.ForegroundColor = ConsoleColor.Green;
                for(int i = 0; i < 25; i++)
                {
                    for (int j = 0; j < 25; j++)
                    {
                        Console.Write('\u25cf');  // '\u25cf'는 동그라미 기호
                    }
                    Console.WriteLine();  // 개행
                }
            }
        }
    }
}
```

30 프레임으로 25 X 25 의 초록색 공을 그리는 코드이다.

- 위 코드의 프레임 관리

  - 30 프레임으로 1초에 30번 그리도록 한다.
  - 경과시간이 1/30 초가 안됐으면 continue
  - 1000/30 밀리세컨즈 = 1/30 세컨즈

- System.Environment.TickCount;
  - 현재 시간을 밀리 세컨즈로 리턴해준다.
  - 1 초 = 1000 밀리 세컨즈
