# Two_Link_Manipulator_SimScape

![image](https://github.com/user-attachments/assets/3ca66b1e-6388-4142-a377-134cea78dbaa)

Рис. 1 – Двухзвенный манипулятор

![Снимок экрана от 2025-06-28 09-28-55](https://github.com/user-attachments/assets/afe60e56-0fbf-46cd-949d-d01bbcb0f5f6)

Вызовем стартовое поле Simscape командой smnew.

![image](https://github.com/user-attachments/assets/e176b086-20a7-4384-9f4b-d3d1307ecff3)

Рис. 2 – Стартовое поле Simscape

Блоки Brick Solid представляют собой прямоугольные параллелепипеды, они выступят в роли плеч манипулятора. Скопируем имеющийся и настроим их параметры в соответствии с исходными данными.

![image](https://github.com/user-attachments/assets/4e4f19f9-6db0-4bc1-9341-485c04cc249e)

Рис. 3 – Первое плечо манипулятора

![image](https://github.com/user-attachments/assets/7e531917-cc64-44cf-8637-75018a243e4f)

Рис. 4 – Второе плечо манипулятора

Для симуляции понадобятся более гибкие настройки инерции. Необходимым параметром будет тензор инерции, рассчитаем его по формуле:

![image](https://github.com/user-attachments/assets/da8cd9b9-c6c9-478c-abe8-6d9013e8dc1e)

Рис. 5 – Используемая методика расчёта тензора инерции

В результате получим:

![image](https://github.com/user-attachments/assets/32fe9dfa-ef13-4c9d-8558-93e7f9879936)

Рис. 6 – Более гибкие настройки первого плеча

![image](https://github.com/user-attachments/assets/1d434b60-6a70-4f1e-ac23-715cd0c7b3d9)

Рис. 7 – Более гибкие настройки второго плеча

Здесь Moments of Inertia представляет собой вектор диагональных элементов тензора инерции, Products of Inertia – вектор недиагональных элементов.
В качестве центра масс укажем начало объекта в связи с особенностями Simscape.
Добавим блоки Rigid Transform – они задают и поддерживают фиксированное пространственное отношение между объектами во время симуляции. Выставим данные блоки перед и после Brick Solid. В качестве оси вращения выберем X. Объединим эти блоки с Brick Solid в подсистемы.

![image](https://github.com/user-attachments/assets/d589f531-1b41-468d-aeb7-fee4d2474983) ![image](https://github.com/user-attachments/assets/bd1f322b-117b-4e07-ae44-eee073782be0)

Рис. 8 – Настройка

![image](https://github.com/user-attachments/assets/e222f8e9-f84c-47e0-a715-76a6fc4b8a65)

Рис. 9 – Подсистемы

![image](https://github.com/user-attachments/assets/b1a2f4d2-7441-46bb-a0a4-2ad42504a70c)

Рис. 10 – Содержимое подсистем

Установим вращающиеся шарниры Revolute Joint для объединения блоков друг с другом. В настройках Revolute Joint выставим определение текущего угла поворота:

![image](https://github.com/user-attachments/assets/598ec5e7-4d4b-47a3-a458-0ec9f0ed06f1)

Рис. 11 – Настройка блока

Подключим Revolute Joint к конвертору PS-Simulink для дальнейшего взаимодействия со стандартными блоками Simulink и подключим его к осциллографу.

![image](https://github.com/user-attachments/assets/f9cd7415-486c-41b0-99ee-adae73e30691)

Рис. 12 – Вывод текущего угла на осциллограф

Добавим цилиндры Cylindrical Solid и объединим с помощью шарниров с плечами манипулятора. В блоках Rigid Transform, прилегающих к блокам Solid, выставим расстояние центра последующего блока Solid от шарнира:

![image](https://github.com/user-attachments/assets/bb87370f-8074-403c-a2ed-3b17fb5bb6ca) ![image](https://github.com/user-attachments/assets/35b4afc3-e539-444e-9145-814425182713)

Рис. 13 – Настройки для первого плеча

![image](https://github.com/user-attachments/assets/a1b07c2d-8c64-4527-b2c5-1d1881b2ac85) ![image](https://github.com/user-attachments/assets/03b82152-391c-49de-81a1-047f88f23cd9)

Рис. 14 – Настройки для второго плеча

Сдвиги центра для обоих цилиндрических блоков равны нулю.
По итогам перечисленных выше действий, схема Simulink должна выглядеть следующим образом:

![image](https://github.com/user-attachments/assets/9278e721-ae00-4bbf-b539-ba3a8927b550)

Рис. 15 – Текущая схема

При запуске мы увидим манипулятор:

![image](https://github.com/user-attachments/assets/3b910a16-11e9-474a-b8c4-35ceef5769cf)

Рис. 16 – Двухзвенный манипулятор

Анимации не последует, потому что манипулятор находится на плоскости XY (это будет именно так, потому что блок Revolute Joint обеспечивает вращение вокруг оси Z).
Для наблюдения свободных колебаний нам потребуется «перевернуть» манипулятор, либо же сместить гравитационные силы с оси Z на ось X илиY (что легко сделать в блоке Mechanism Configuration). Перевернём манипулятор на 90 градусов по оси Y:

![image](https://github.com/user-attachments/assets/f9581f25-fac3-48f3-ad5f-7656860aa960)

Рис. 17 – Итоговая схема

Последний важный шаг – коэффициент затухания. Он настраивается в блоках Revolute Joint:

![image](https://github.com/user-attachments/assets/023eb33e-afb6-49bf-b184-018b9b19355f)

Рис. 18 – Настройка коэффициента затухания в шарнирах перед плечами манипулятора

Настроить начальное положение манипулятора мы также можем в тех же двух блоках Revolute Joint:

![image](https://github.com/user-attachments/assets/c62f2e46-8deb-4fb4-a152-c71878ded135)

Рис. 19 – Настройка начального положения плеч манипулятора

Для начального положения  и  получились следующие графики изменения углов:

![image](https://github.com/user-attachments/assets/0c6260a1-925b-4088-ab5f-5647d377639f)

Рис. 20 – Первое плечо

![image](https://github.com/user-attachments/assets/6eef09d6-714f-4b8a-b956-46e1c5fc496f)

Рис. 21 – Второе плечо

При изменении некоторых параметров в симуляции (таких, как коэффициент затухания) модель двухзвенного манипулятора может больше соответствовать поведению реального объекта.
