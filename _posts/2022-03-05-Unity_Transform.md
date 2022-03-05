---
title: "Chapter 3. Transform"
excerpt:

categories:
  - Unity Part3
tags:
  - [Unity, Game, Engine]

toc: true
toc_sticky: true

date: 2022-03-05
last_modified_at: 2022-03-05
---

## Scale

- 오브젝트의 크기를 확대/축소할 때 사용한다

Scale 사용 팁 : 거대한 모델을 필요하다고 가정하면, 모델을 만들 때 처음부터 거대한 모델을 만들 필요 없이 모델은 작게 만들어주고 유니티에 와서 Scale을 통해 크기를 늘리면 된다.

## Position

월드 공간에서 트랜스폼의 위치를 나타낸다

```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class PlayerController : MonoBehaviour
{
    [SerializeField]
    private float _speed = 10.0f;

    void Start()
    {

    }

    void Update()
    {
        if (Input.GetKey(KeyCode.W))
            transform.position += new Vector3(0.0f, 0.0f, 1.0f) * Time.deltaTime * _speed;
        if (Input.GetKey(KeyCode.S))
            transform.position -= new Vector3(0.0f, 0.0f, 1.0f) * Time.deltaTime * _speed;
        if (Input.GetKey(KeyCode.D))
            transform.position += new Vector3(1.0f, 0.0f, 0.0f) * Time.deltaTime * _speed;
        if (Input.GetKey(KeyCode.A))
            transform.position -= new Vector3(1.0f, 0.0f, 0.0f) * Time.deltaTime * _speed;
    }
}
```

- W키를 누르는 중이라면 현재 위치로부터 한 프레임동안 Vector3(0.0f, 0.0f, Time.deltatime \* \_speed) 만큼 더 이동한다
  - 방향(Vergtor) = Vector3(0.0f, 0.0f, 1.0f)
    - Z축 positive이므로 `Vector3(0.0f, 0.0f, 1.0f) == Vector3.forword`
  - 거리 = 시간 x 속력 = Time.deltaTime \* \_speed;
    - 프레임의 간격 시간동안 \_speed 만큼 이동한다
- 나머지 방향으로도 같은 원리로 동작한다
  - Z축 negative 방향 = Vector3.back
  - X축 positive 방향 = Vector3.right
  - X축 negative 방향 = Vector3.left

```cs
    void Update()
    {
        if (Input.GetKey(KeyCode.W))
            transform.position += Vector3.forward * Time.deltaTime * _speed;
        if (Input.GetKey(KeyCode.S))
            transform.position += Vector3.back * Time.deltaTime * _speed;
        if (Input.GetKey(KeyCode.D))
            transform.position += Vector3.right * Time.deltaTime * _speed;
        if (Input.GetKey(KeyCode.A))
            transform.position += Vector3.left * Time.deltaTime * _speed;
    }
```

따라서 위의 Update 함수를 위와 같이 변경할 수 있다.
