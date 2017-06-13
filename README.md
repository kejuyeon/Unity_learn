# Unity Learn

<br/>
<br/>
<br/>

## Install
* https://store.unity.com/kr/download?ref=personal

<br/>
<br/>


## Background


### Background scrolling `ScrollingObject.cs`

```c#
using UnityEngine;
using System.Collections;

public class ScrollingObject : MonoBehaviour {

	private Rigidbody2D rd2d;


	void Start () {
		rd2d = GetComponent<Rigidbody2D>();
		rd2d.velocity = new Vector2(-1.5f, 0);
	}


	void Update () {
	
		// gameover 일때 멈춤
	}
}

```

### Background Repeat `RepeatObject.cs`

```c#
using UnityEngine;
using System.Collections;

public class RepeatObject : MonoBehaviour {

	BoxCollider2D groundCollider;
	float horizontalLength;
	
	void Start () {
		groundCollider = GetComponent<BoxCollider2D>();
		horizontalLength = groundCollider.size.x;
	}
	
	void Update () {
		
		// 현재position.x 값이 box의 width 값보다 작을때 이동시켜줌
		if(transform.position.x < -groundHorizontalLength) {

			Vector2 backgroundOffset = new Vector2(groundHorizontalLength * 2f, 0);
			transform.position = (Vector2)transform.position + backgroundOffset;
		}
	}
}
```



## Bird


### bird Setting `BirdController.cs`

```c#
using UnityEngine;
using System.Collections;

public class BirdController : MonoBehaviour {

	Rigidbody2D bird;
	bool isDead = false;
	float upForce = 200f;

	void Start () {
		bird = GetComponent<Rigidbody2D>();
	}
	
	void Update () {
		if (isDead == false) {

			if(Input.GetKeyDown("space")) {

				bird.velocity = Vector2.zero;
				// jump
				bird.AddForce(new Vector2(0, upForce));
			}
		}
	}

	// Sent when an incoming collider makes contact with this object's collider (2D physics only).
	// 객체 충돌할때 호출
	void OnCollisionEnter2D(Collision2D other) {
		isDead = true;
	}
}
```

### Bird Animation

- **`anim.SetTrigger("Jump");`**  **`anim.SetTrigger("Die");`** 추가

```c#
using UnityEngine;
using System.Collections;

public class BirdController : MonoBehaviour {

	Rigidbody2D bird;
	Animator anim;
	bool isDead = false;
	float upForce = 200f;

	void Start () {
		bird = GetComponent<Rigidbody2D>();
		anim = GetComponent<Animator>();
	}
	
	void Update () {
		if (isDead == false) {

			if(Input.GetKeyDown("space")) {

				bird.velocity = Vector2.zero;
				// jump
				bird.AddForce(new Vector2(0, upForce));
				// animation 추가
				anim.SetTrigger("Jump");
			}
		}
	}

	// Sent when an incoming collider makes contact with this object's collider (2D physics only).
	// 객체 충돌할때 호출
	void OnCollisionEnter2D(Collision2D other) {
		isDead = true;
		// animation 추가
		anim.SetTrigger("Die");
	}
}

```



