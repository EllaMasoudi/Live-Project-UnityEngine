# Live Project

In two weeks internship I redesigned an arcade game in Unity using C#. There are some code snippet from my game in this repository 

# Player Controller Script
In this script  

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
