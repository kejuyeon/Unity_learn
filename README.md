# Unity Learn

<br/>
<br/>
<br/>

## Install
* https://store.unity.com/kr/download?ref=personal

<br/>
<br/>


## Background

### Background scrolling

```
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

### Background Repeat
```
using UnityEngine;
using System.Collections;

public class RepeatObject : MonoBehaviour {

	BoxCollider2D groundCollider;
	float horizontalLength;

	// Use this for initialization
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

