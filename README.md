# Live Project

In two weeks internship I redesigned an arcade game in Unity using C#. There are some code snippet from my game in this repository 

# Player Controller Script
This script shows the score on the canvas. The player movement defined here. Also if the player collides with Rock and an enemy in the game will destroy.

        void Start()
        {

            textScore = GameObject.Find("textScoreinCanvas").GetComponent<Text>();
            textScore.text = "Score: " + score.ToString();
        }
        void Update()
        {
      
            if (Input.GetKey(KeyCode.W) && !isMoving)
                StartCoroutine(MovePlayer(Vector3.up));

            if (Input.GetKey(KeyCode.A) && !isMoving)
                StartCoroutine(MovePlayer(Vector3.left));


            if (Input.GetKey(KeyCode.S) && !isMoving)
                StartCoroutine(MovePlayer(Vector3.down));

            if (Input.GetKey(KeyCode.D) && !isMoving)
                StartCoroutine(MovePlayer(Vector3.right));
        }
        private void FixedUpdate()
        {
            if (isMoving && !p_FacingRight)
            {
                Flip();
            }
            else if(isMoving && p_FacingRight)
            {
                Flip();
            }
        }

        private IEnumerator MovePlayer(Vector3 direction)
        {
            isMoving = true;
            float elapsedTime = 0;

            origPos = transform.position;
            targetPos = origPos + direction;

            while (elapsedTime < timeToMove)
            {
                transform.position = Vector3.Lerp(origPos, targetPos, (elapsedTime / timeToMove));
                elapsedTime += Time.deltaTime;
                yield return null;

            }
            transform.position = targetPos;
            isMoving = false;
        }

        private void OnTriggerEnter2D(Collider2D other)
        {
            if (other.name == "Rock")
            {
                this.gameObject.SetActive(false);
                Destroy(gameObject);
                transform.GetComponent<Rigidbody2D>().constraints = RigidbodyConstraints2D.FreezePositionX |
                RigidbodyConstraints2D.FreezePositionX;
                SceneManager.LoadScene(21);
            }
            if (other.name == "Enemy")
            {
                Destroy(gameObject);

            }
        }
        private void Flip()
        {
            p_FacingRight = !p_FacingRight;
            transform.Rotate(0f, 180f, 0f);
        }

    }

    }
    
 # Script for the Rock 
 In this script , if the player destroys the ground(rock support) under the rock then the rock will fall down. If the rock collides with the player will destroy the player. 
 
    void Start()
    {
        GetComponent<PolygonCollider2D>().enabled = false;
    }

    void Update()
    {
        GetComponent<PolygonCollider2D>().enabled = true;

        if (GameFlow.supportGone == "y")
        {
            StartCoroutine(dropRock());
            GameFlow.supportGone = "n";
        }
    }
    void OnTriggerEnter2D(Collider2D other)
    {
        if (other.name == "BottomBorder")
        {
            
            Destroy(gameObject);
        }
        else if (other.name == "Player")
        {
            Debug.Log("score reduced!!");
            
        }
    }

    IEnumerator dropRock()
    {
        
        yield return new WaitForSeconds (3);
        GetComponent<Rigidbody2D>().gravityScale = 1;
    }

}
