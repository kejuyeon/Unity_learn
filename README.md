# Unity Learn

<br/>
<br/>
<br/>

## Install
* https://store.unity.com/kr/download?ref=personal

<br/>
<br/>


## Background


* ### Background scrolling 
`ScrollingObject.cs`

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

* ### Background Repeat 
`RepeatObject.cs`

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

<br/>
<br/>

## Bird


* ### bird Setting 
`BirdController.cs`

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

* ### Bird Animation

`BirdController.cs`
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

<br/>
<br/>

## Text

* ### ScoreText


`GameController.cs`

```c#
using UnityEngine;
using System.Collections;
using UnityEngine.UI;
using UnityEngine.SceneManagement;

public class GameController : MonoBehaviour {

	public static GameController instance;

	public bool gameOver = false;
	int score = 0;
	public GameObject gameOverText;
	public Text scoreText;

	void Awake() {

		if(instance == null) {
			instance = this;
		} else if (instance != this) {
			Destroy(gameObject);
		}
	}
	
	void Update () {

		if (gameOver && Input.GetKeyDown(KeyCode.R)) {
			// restart
			SceneManager.LoadScene(SceneManager.GetActiveScene().buildIndex);
		}
	}

	public void AddScore() {
		if(gameOver == false) {
			score++;
			scoreText.text = "SCORE : " + score.ToString();
		}
	}

	public void GetGameOver() {
		// Text 보여주기
		gameOverText.SetActive(true);
		gameOver = true;
	}
}
```

<br/>

`BirdController.cs`

```c#
...
	void OnCollisionEnter2D(Collision2D other) {
		isDead = true;
		// animation 추가
		anim.SetTrigger("Die");
		
		GameController.instance.GetGameOver();
	}
...
```

<br/>

`ScrollingObject.cs`

```c#
...
	void Update () {
	
		// gameover 일때 멈춤
		if( GameController.instance.gameOver == true ) {
			background.velocity = Vector2.zero;
		}
	}
```


<br/>
<br/>

## Pipes

* ### pipe 충돌 처리

<br/>

`PipesController.cs`

```c#
using UnityEngine;
using System.Collections;

public class PipesController : MonoBehaviour {

	void OnTriggerEnter2D(Collider2D other) {
		// 통과하면 점수 추가
		if (other.GetComponent<BirdController>() != null) {
			GameController.instance.AddScore();
		}
	}
}
```


### pipe 반복처리

> - Time.deltaTime
>The time in seconds it took to complete the last frame (Read Only).

<br/>

`PipesPool.cs`

```c#
using UnityEngine;
using System.Collections;

public class PipesPool : MonoBehaviour {

	public GameObject perfabs;
	GameObject[] pipes;

	int pipesPoolSize = 5;
	Vector2 objectPosition = new Vector2(-15f, -30f);
	int current = 0;
	float lastTime = 2f;

	void Start () {
	
		// pipe 저장하기
		pipes = new GameObject[pipesPoolSize];

		for(int i = 0; i < pipesPoolSize; i++) {
			pipes[i] = (GameObject) Instantiate(perfabs, objectPosition, Quaternion.identity);
		}
	}
	
	void Update () {
	
		lastTime += Time.deltaTime;

		if(GameController.instance.gameOver == false && lastTime >= 4f) {

			// 파이프 짠
			float YPosition = Random.Range(-1.5f, 2f);
			pipes[current].transform.position = new Vector2(5f, YPosition);

			current++;
			lastTime = 0;

			if (current >= pipesPoolSize) {
				current = 0;
			}
		}
	}
}

```

