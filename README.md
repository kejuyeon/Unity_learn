# Unity Learn

## Install
* https://store.unity.com/kr/download?ref=personal


## Asset 추가하기

* Window > Asset Store 에서 검색 Space Shooter > 추가
```
![map](./README/map.png)
```



## 배경 만들기

* GameObject > 3D Object > Quad 생성

* Quad 이름 Background 로 변경

* Mesh Render 의 Material 속성에 `Textures > done_tile_nebula_green_dff` 속성 안으로 드래그해서 추가

* `scale x : 15, y : 30` 수정

* MainCamera `y : 10 rotation x : 90` 수정

* Prefabs > VFX > Starfield 파일 드래그해서 추가

```
![map](./README/map.png) 
```



## 우주선 만들기

* vehicle_playerShip 드래그해서 추가
* PlayerController C# 스트립트 생성
* vehicle_playerShip 에 스트립크 연결

### PlayerController.cs

방향키로 우주선 움직이도록 스크립트 수정

```
using UnityEngine;
using System.Collections;

public class PlayerController : MonoBehaviour {

	float speed = 5;

	// Use this for initialization
	void Start () {
	
	}

	void FixedUpdate() {
		float x = Input.GetAxis("Horizontal");
		float y = Input.GetAxis("Vertical");

		Vector3 movement = new Vector3(x, 0, y);

		GetComponent<Rigidbody>().velocity = movement * speed;
		GetComponent<Rigidbody>().rotation = Quaternion.sEuler(0, 0, GetComponent<Rigidbody>().velocity.x * -5);
	}
}

```


* Rigidbody
물리 시뮬레이션을 통해서 오브젝트의 위치를 조절합니다.
리지드바디(rigidbody) 컴포넌트는 오브젝트의 위치를 제어합니다. - 중력의 영향에 의해 오브젝트를 아래로 떨어지도록 만들고,
충돌에 대한 오브젝트의 반응의 크기를 계산할 수 있습니다.

  * velocity : 리지드바디의 속력 벡터를 나타냅니다.

  * rotation : 리지드바디의 회전을 나타냅니다.


* Quaternion
쿼터니언(Quaternions)은 회전을 표현하기 위해 사용됩니다.

  * Euler : z축 주위로 z, x축 주위로 x, y축 주위로 y 각도만큼 회전한(순서대로) Rotation을 반환합니다.



### 우주선이 화면 밖으로 넘어가지 않도록 수정

Create Empty 추가
