---
## Front matter
lang: ru-RU
title: Презентация по лабораторной работе №7
subtitle: Эффективность рекламы  
author:
  - Ибатулина Д.Э.
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 17 мая 2025

## i18n babel
babel-lang: russian
babel-otherlangs: english

## Formatting pdf
toc: false
toc-title: Содержание
slide_level: 2
aspectratio: 169
section-titles: true
theme: metropolis
header-includes:
 - \metroset{progressbar=frametitle,sectionpage=progressbar,numbering=fraction}
---

# Информация

## Докладчик

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

  * Ибатулина Дарья Эдуардовна
  * студентка группы НФИбд-01-22
  * Российский университет дружбы народов
  * [1132226434@rudn.ru](mailto:1132226434@rudn.ru)
  * <https://deibatulina.github.io>

:::
::: {.column width="30%"}

![](./image/me.jpg)

:::
::::::::::::::

# Введение

## Цель работы

Исследовать модель эффективности рекламы. 

## Задание

Построить график распространения рекламы, математическая модель которой описывается
следующим уравнением:

1. $\dfrac{dn}{dt} = (0.77+0.000075n(t))(N-n(t))$

2. $\dfrac{dn}{dt} = (0.000075+0.77n(t))(N-n(t))$

3. $\dfrac{dn}{dt} = (0.2*cos(t)+0.7*cos(t)n(t))(N-n(t))$

При этом объем аудитории $N = 1203$, в начальный момент о товаре знает 15 человек. Для случая 2 определить в какой момент времени скорость распространения рекламы будет иметь максимальное значение.

# Выполнение лабораторной работы

## Реализация на Julia

```Julia
using DifferentialEquations, Plots;
f(n, p, t) = (p[1] + p[2]*n)*(p[3] - n)
p1 = [0.77, 0.000075, 1203]
p2 = [0.000075, 0.77, 1203]
n_0 = 15
tspan1 = (0.0, 15.0)
tspan2 = (0.0, 0.02)
prob1 = ODEProblem(f, n_0, tspan1, p1)
prob2 = ODEProblem(f, n_0, tspan2, p2)
```

## Реализация на Julia

```Julia
sol1 = solve(prob1, Tsit5(), saveat = 0.01)
plot(sol1, markersize =:15, c =:green, yaxis = "N(t)")
```

## Реализация на Julia

![График распространения рекламы для случая 1](image/1.png){#fig:001 width=70%}

## Реализация на Julia

```Julia
sol2 = solve(prob2, Tsit5(), saveat = 0.0001)
plot(sol2, markersize =:15, c=:green, yaxis="N(t)")
```

## Реализация на Julia

![График распространения рекламы для случая 2](image/2.png){#fig:002 width=70%}

## Реализация на Julia

```Julia
function f3(u,p,t)
    n = u
    dn = (0.2*cos(t) + 0.7*cos(t)*n)*(1203 - n)
end
u_0 = 15
tspan = (0.0, 2)
prob = ODEProblem(f3, u_0, tspan)
sol = DifferentialEquations.solve(prob, Tsit5(), saveat = 0.001)
plot(sol, markersize =:15, c=:green, yaxis="N(t)")
```

## Реализация на Julia

![График распространения рекламы для случая 3](image/3.png){#fig:003 width=70%}

## Реализация на OpenModelica

```
 model lab7_1
  parameter Real a_1 = 0.77;
  parameter Real a_2 = 0.000075;
  parameter Real N = 1203;
  parameter Real n_0 = 15;
  
  Real n(start=n_0);

equation
  der(n) = (a_1 + a_2*n)*(N - n);

end lab7_1;
```

## Реализация на OpenModelica

![График распространения рекламы для случая 1](image/4.png){#fig:004 width=70%}

## Реализация на OpenModelica

```
model lab7_2
  parameter Real a_1 = 0.000075;
  parameter Real a_2 = 0.77;
  parameter Real N = 1203;
  parameter Real n_0 = 15;
  
  Real n(start=n_0);

equation
  der(n) = (a_1 + a_2*n)*(N - n);

end lab7_2;
```

## Реализация на OpenModelica

![График распространения рекламы для случая 2](image/5.png){#fig:005 width=70%}

## Реализация на OpenModelica

![График изменения производной с течением времени](image/6.png){#fig:006 width=70%}

## Реализация на OpenModelica

![Максимальное значение скорости распространения рекламы](image/7.png){#fig:007 width=70%}

## Реализация на OpenModelica

```
model lab7_3
  parameter Real a_1 = 0.2;
  parameter Real a_2 = 0.7;
  parameter Real N = 1203;
  parameter Real n_0 = 15;
  
  Real n(start=n_0);

equation
  der(n) = (a_1*cos(time) + a_2*cos(time)*n)*(N - n);

end lab7_3;
```

## Реализация на OpenModelica

![График распространения рекламы для случая 3](image/8.png){#fig:008 width=70%}

# Выводы

В результате выполнения данной лабораторной работы была исследована модель эффективности рекламы.
