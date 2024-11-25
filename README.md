
## Комп'ютерні системи імітаційного моделювання
## СПм-23-3, **Козін Микита Дмитрович**
### Лабораторна робота №2. Модифікація моделей у NetLogo

### Варіант 8: модель у середовищі NetLogo
[Segregation Simple Extension 1](http://www.netlogoweb.org/launch#http://www.netlogoweb.org/assets/modelslib/IABM%20Textbook/chapter%203/Segregation%20Extensions/Segregation%20Simple%20Extension%201.nlogo). Додано можливість зникнення агента залежно від кількості "чужих" та їх частки в оточенні. Результати відображаються графічно.

### Зміни у вихідній моделі за варіантом:
- Додано змінну *turtles-own* для підрахунку "ворогів поблизу":
<pre>
  enemy-nearby   ;; кількість чужих серед сусідніх черепах
</pre>

- Оновлено процедуру **update-turtles** для розрахунку кількості ворогів:
<pre>
  set enemy-nearby (total-nearby - similar-nearby)
</pre>

- Додано розрахунок відсоткового співвідношення союзників та ворогів:
<pre>
set ally-percent (ifelse-value total-nearby != 0 [((similar-nearby / total-nearby) * 100)] [0])
set enemy-percent (ifelse-value total-nearby != 0 [((enemy-nearby / total-nearby) * 100)] [0])
</pre>

- Введено змінну *surrounded?*, яка визначає, чи агент оточений ворогами:
<pre>
set surrounded? ally-percent < enemy-percent
</pre>

- Реалізовано логіку видалення оточених агентів у процедурі **kill-surrounded**:
<pre>
to kill-surrounded
  ask turtles with [ surrounded? ] [
    if random 10 = 1 [ die ]  ;; 10% шанс зникнення черепахи
  ]
end
</pre>

- Додано графік динаміки популяції та знімки екрана моделі.

### Додаткові зміни:
- Введено лічильники для:
  - Загальної кількості черепах.
  - Кількості вбивств.
  - Кількості черепах за кольорами.

## Обчислювальні експерименти
### 1. Вплив бажаної кількості схожих агентів на кількість зникнень
Проаналізовано залежність **Killed** від **%-similar-wanted**. Параметри:
- **number-of-ethnicities** = 5
- **number** = 2000

| %-similar-wanted | Кількість вбивств |
|------------------|-------------------|
| 10               | 896               |
| 25               | 504               |
| 50               | 515               |
| 75               | 1344              |
| 100              | 1677              |

Результати вказують на те, що надто низькі або високі значення параметра призводять до зростання втрат серед агентів.
