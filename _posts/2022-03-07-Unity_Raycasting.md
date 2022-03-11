---
title: "Chapter 3. Raycasting"
excerpt:

categories:
  - Unity Part3
tags:
  - [Unity, Game, Engine]

toc: true
toc_sticky: true

date: 2022-03-07
last_modified_at: 2022-03-07
---

## Raycast

레이저를 쏴서 충돌되는 물체가 있는지 없는지를 검사하는 것이 Raycasting이다.

예를 들어 게임 속 물체를 클릭한다는 것은 클릭한 화면의 위치로부터, 즉 카메라의 위치로부터 레이저(Raycast)를 쏴서 닿은 물체를 활성화시키는 것과 같다.

- 3인칭 카메라에서 플레이어를 향해 Raycast를 쐈을 때 장애물이 있다면 -> 플레이어가 장애물에 가려지지 않도록 카메라 위치를 장애물 넘어로 이동시킴
- 롤에서 화면에 마우스로 찍은 위치에서 카메라가 Raycast를 월드 좌표로서 쏴서 그 곳으로 캐릭터를 이동시키게도 할 수 있다.

```cs
    void Update()
    {
        // 캐릭터가 움직이는 방향에 따라 Raycast도 움직이도록 forward를 로컬 좌표로 인식하고 거기에 해당하는 월드 좌표를 찾으면 된다
        // 로컬 좌표로서의 (0, 0, 1)을 월드 좌표로 변환하여 리턴
        Vector3 look = transform.TransformDirection(Vector3.forward);

        Debug.DrawRay(transform.position, look * 10, Color.red);
        // transform.position 에서 빨강색 광선을 (transform.position + look * 10) 위치까지 쏜다.
        // Debug.DrawRay는 start 지점부터 start + dir 지점까지 선을 그어주므로 두번째 인자에 방향 뿐만 아니라 크기도 있어야 한다

        if (Physics.Raycast(transform.position + Vector3.up, look))
        {
            Debug.Log("Raycast!");
        }

        // maxDistance 추가
        if(Physics.Raycast(transform.position + Vector3.up, look, 20))
        {
            Debug.Log("Raycast!");
        }

        // Raycasting 대상을 알기 위해서 RaycastHit 추가
        RaycastHit hit;
        if(Physics.Raycast(transform.position + Vector3.up, look, out hit, 20))
        {
            Debug.Log($"Raycast {hit.collider.gameObject.name}!");
        }
    }
```

```cs
Physics.Raycast(transform.position + Vector3.up, look, out hit, 20)
```

- transform.position + Vector3.up 위치로부터 광선을 쏜다.
  - Vector3.up을 더해준 이유는 이 스크립트의 주인 오브젝트의 Pivot이 땅바닥에 있어서 조금 위에서 광선을 쏘기 위해 (0, 1, 0)만큼을 더해줌
- look 방향으로 광선을 쏜다.
  - look = transform.TransformDirection(Vector3.forward)
  - 로컬 좌표(0, 0, 1)을 월드 좌표로 변환
  - 스크립트의 주인 오브젝트 기준에서의 앞 방향이 월드 좌표계로 변환된 것
    - 광선의 시작 위치와 방향을 월드 좌표 기준으로 설정해 주어야 함
- Raycasting 대상 오브젝트의 정보는 hit에 담긴다.
  - position, normalized 등등 충돌한 오브젝트의 위치 방향 등의 정보
- 광선을 20 만큼의 길이로 쏜다.

Raycasting은 하나의 물체와 충돌하면 더 이상 관통하지 않는다. Raycasting의 일직선상에 있는(관통하는) 모든 물체를 알고싶으면 RaycastAll을 사용한다.

```cs
    void Update()
    {

        Vector3 look = transform.TransformDirection(Vector3.forward);

        RaycastHit[] hits;
        hits = Physics.RaycastAll(transform.position + Vector3.up, look, 20);

        foreach (RaycastHit hit in hits)  // 관통되서 광선에 닿은 모든 물체들이 hits 배열에 담긴다. for문으로 모든 원소에 접근
        {
            Debug.Log("Raycast!");
        }
    }
```

Raycast와는 다르게 관통된 모든 오브젝트를 배열로 리턴한다.

- 위에서는 maxDistance를 20으로 지정해서 더 멀리 있는 객체까지는 닿지 않는다.

## 투영

롤에서 캐릭터를 이동시킬때 화면에 마우스 우클릭을 하는 경우에는 어떤 좌표를 기준으로 레이저를 쏘아야 하는지 난감하다.

이를 이해하기 위해서는 좌표계를 심화해서 이해할 필요가 있다.

- Local : 특정 물체를 기준으로 한 좌표계
- World : 전체가 공유하는 하나의 공통된 좌표계. 게임 세상의 좌표계가 월드 좌표계이다.

월드에 물체가 있다 하더라도 최종적으로 화면에 들어와야 하는데 화면은 2d 좌표이다. 3d 좌표를 2d 좌표로 변환하는 방법이 필요하다.

### Screen 좌표계

Screen 좌표를 얻기 위해서 Input.mousePosition를 사용한다. 현재 마우스 좌표를 픽셀 좌표로 뽑아온다. 픽셀 좌표가 Screen 좌표이다.

```cs
void Update
{
  Debug.Log(Input.mousePosition);
}
```

- 마우스가 왼쪽 하단으로 가면 (0, 0, 0)에 가까워 지고 오른쪽 위로 가면 숫자가 커진다.
  - (x, y, z)에서 x, y만 커지고 z는 0으로 인데 2d 좌표이므로 z값은 없다. 해상도가 1960 X 1080이라면 (1960, 1080, 0)이 Max값이다.
- 화면에 표시하는 모니터의 픽셀은 한 점을 어떤 색상으로 표현할지를 의미하는데 각 픽셀마다 하나의 좌표가 대응한다고 보면 된다.

### Viewport

Viewport 좌표는 Camera.main.ScreenToViewportPoint를 사용해서 구한다.

```cs
void Update
{
  Debug.Log(Camera.main.ScreenToViewportPoint(Input.mousePosition));
}
```

Viewport 좌표는 Screen 좌표와 유사한다 단지 비율로 표시해 주는 차이만 있다. Viewport 좌표는 좌측 하단으로 가면 (0, 0, 0)에 가까워지고 우측 상단으로 가면 (1, 1, 0)에 가까워 진다.

- Screen 좌표는 픽셀 좌표를 기준으로 함
- Viewport 좌표는 픽셀과 상관없이 화면 비율에서 몇프로를 차지하는지에 대해 표현

Screen 좌표와 Viewport 좌표는 비슷하다고 볼 수 있다.

특정 Screen 좌표를 알았을 때 World 좌표를 어떻게 알아낼 수 있을까?
