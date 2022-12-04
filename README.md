Выполнил: Смирнов Н.М

Группа: ЭВТ-70

Игровой движок: Unity 2021.3.15

Название работы: разработка игры Circle

Лабораторная работа 24

Тема: разработка игры Circle

Цель: разработать игру Circle

Оборудование: компьютер.

Ход работы

1.Выполнение работы

Корректируем спрайт Круга чтобы он был полноценным, после добавляем спрайт линии.

![image](https://user-images.githubusercontent.com/119733911/205502749-fc8e3982-ab4d-4441-ae83-664e18ad18bb.png)

Рисунок 24.1 Добавили спрайты

Раставили линии в произвольном порядке и ставим сверху и снизу круга Box Collider и если эти коллайдеры докоснутся до линии, то игра закончится.

![image](https://user-images.githubusercontent.com/119733911/205502836-4f2bc6af-76a0-4c76-9deb-69c35cd3f7a5.png)

Рисунок 24.2 Раставили линии и сделали коллайдеры на Circle

Листинг 24.1 Player.cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
public class Player : MonoBehaviour
{
    public Vector2 jumpForce;
    Vector2 currentVelocity;
    Rigidbody2D rgbd;
    GameManager gameManager;
    ScoreUI scoreUI;

    void Start()
    {
        rgbd = GetComponent<Rigidbody2D>();
        rgbd.gravityScale = 0;
        gameManager = FindObjectOfType<GameManager>();
        scoreUI = FindObjectOfType<ScoreUI>();
    }
    void Update()
    {
        if (gameManager.gameOver) { rgbd.bodyType = RigidbodyType2D.Static;return; }
        if (Input.GetMouseButtonDown(0))
        {
            if (rgbd.gravityScale != 0.5f) { rgbd.gravityScale = 0.5f; }
            rgbd.AddForce(jumpForce);
            SpeedController();
            scoreUI.IncrementScore(1);
        }
    }
    void SpeedController()
    {
        currentVelocity = rgbd.velocity;
        currentVelocity.x = Mathf.Clamp(currentVelocity.x, 2, 2);
        currentVelocity.y = Mathf.Clamp(currentVelocity.y, 2, 2);
        rgbd.velocity = currentVelocity;
    }
}

Листинг 24.2 CameraController.cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class CameraController : MonoBehaviour
{
    public Transform playerTransform;
    void Start()
    {
        
    }

    void Update()
    {
        transform.position = new Vector3(playerTransform.position.x, transform.position.y, -10);
    }
}

Листинг 24.3 Block.cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Block : MonoBehaviour
{
    public void OnTriggerEnter2D(Collider2D collision)
    {
        if(collision.tag == "Player")
        {
            Debug.Log("hit by player");
            FindObjectOfType<GameManager>().gameOver = true;
        }
    }
}

Листинг 24.4 Diamond.cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Diamond : MonoBehaviour
{
    public void OnTriggerEnter2D(Collider2D collision)
    {
        if(collision.tag == "Player")
        {
            //add points
            FindObjectOfType<ScoreUI>().IncrementDiamond(1);
            FindObjectOfType<ScorePointCanvas>().DiamondHit(collision.transform.position);
            Destroy(gameObject);
        }
    }
}


Листинг 24.5 ScoreUI.cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class ScoreUI : MonoBehaviour
{
    int Score,Diamond;
    public TMPro.TextMeshProUGUI ScoreText;
    public TMPro.TextMeshProUGUI DiamondText;

    public void IncrementScore( int value)
    {
        Score += value;
        UpdateDisplay();
    }

    public void IncrementDiamond(int value)
    {
        Diamond += value;
        UpdateDisplay();
    }

    private void UpdateDisplay()
    {
        ScoreText.text = Score.ToString();
        DiamondText.text = Diamond.ToString();
    }
}

2.Вывод:
В ходе проделанной работы, мы разработали игру Circle.

