## Комп'ютерні системи імітаційного моделювання
## СПм-23-4, **Іванченко Даниїл Ігорович**
### Лабораторна робота №**2**. Редагування імітаційних моделей у середовищі NetLogo

<br>

### Варіант 8, модель у середовищі NetLogo:
[Segregation Simple Extension 1](http://www.netlogoweb.org/launch#http://www.netlogoweb.org/assets/modelslib/IABM%20Textbook/chapter%203/Segregation%20Extensions/Segregation%20Simple%20Extension%201.nlogo), Додати можливість агенту зникнути, вірогідність чого залежить від кількості наявних поряд з ним "чужих" і кількості представників їхньої групи у їх власному оточенні. Виводити графік зміни розміру популяції.

<br>

### Внесені зміни у вихідну логіку моделі, за варіантом:

Додано нову змінну *turtles-own*:
<pre>
  enemy-nearby   ;; how many enemy patches have a turtle?
</pre>
Та розроблено логіку розрахунку у процедурі **update-turtles**:
<pre>
  ;; count enemies
  set enemy-nearby (total-nearby - similar-nearby)
</pre>
Додано змінні *turtles-own*:
<pre>
  ally-percent
  enemy-percent
</pre>
Та розроблено логіку підрахунку процентажу союзників та противників у процедурі **update-turtles**:
<pre>
;; percentage allies
set ally-percent (ifelse-value total-nearby != 0 [((similar-nearby / total-nearby) * 100)] [0])
;; percentage enemies
set enemy-percent (ifelse-value total-nearby != 0 [((enemy-nearby / total-nearby) * 100)] [0])
</pre>
Додано змінну *turtles-own*:
<pre>
surrounded?
</pre>
Задіяв створені раніше змінні для обчислення значення *оточеності*:
<pre>
set surrounded? ally-percent < enemy-percent
</pre>
Та розроблено логіку вбивства оточених черепах у новій процедурі **kill-surrounded**:
<pre>
to kill-surrounded
  ask turtles with [ surrounded? ] [
    if random 10 = 1 [ die ]       ;; 10% chance that surrounded turtle will be killed
  ]
end
</pre>
Додано графік зміни розміру популяції:

![Графік 1](Графік1.png)

Скріншот моделі в процесі симуляції:

![Скріншот 1](Скріншот1.png)

<br>

### Внесені зміни у вихідну логіку моделі, на власний розсуд:
Додано лічильник черепах:

![Лічильник черепах](Лічильник_черепах.png)

Додано підрахунок вбивств:

![Лічильник вбивств](Лічильник_вбивств.png)
<pre>
set count-kill number - count turtles
</pre>

А також лічильники кожного кольору:

![Лічильники кольорів](Лічильники_кольорів.png)

Скріншот моделі в процесі симуляції:

![Скріншот 2](Скріншот2.png)

<br>

## Обчислювальні експерименти
### 1. Вплив бажаної кількості однакових черепах на кількість вбивств
Досліджується залежність показника **Killed** від керуючого параметру **%-similar-wanted**.
Значення керуючих параметрів за замовчуванням:
- **number-of-ethnicities** 5
- **number** 2000

<table>
<thead>
<tr><th>%-similar-wanted</th><th>Кількість вбивств</th></tr>
</thead>
<tbody>
<tr><td>10</td><td>896</td></tr>
<tr><td>25</td><td>504</td></tr>
<tr><td>50</td><td>515</td></tr>
<tr><td>75</td><td>1344</td></tr>
<tr><td>100</td><td>1677</td></tr>
</tbody>
</table>

![Графік 2](Графік2.png)

Графік чітко демонструє, що при занадто низькому значенні кількості необхідних союзників бродячі вороги випадково оточують і знищують черепах. У той же час, при надто високих параметрах цього показника зростає ймовірність того, що черепаха, блукаючи, потрапить у пастку, де її також оточують і вбивають.
