#### бросание монеты
$X=\{x_1,x_2,x_3..x_N\}$, где $x= 0,1$ (x может принимать только 2 значения 1, 0 ) . $X$ - наша выборка полученная в ходе эксперимента \
по бросанию монеты. Пусть мы считаем что наша модель описывается распределением Бернулли где $x=0,1$: 

$$ P_q(x)=q^x(1-q)^{1-x} ;$$

Тогда вероятность получить нашей выборку такой какой мы ее имеем равна \
(представим что хотим посмотеть на сколько вероятен результат нашего эксперимента при разных параметрах $q$): 

$$P_q(X)=P_q(x=x_1)P_q(x=x_2)...P_q(x=x_N)$$

Вот $P_q(X)$ и есть лайклихуд. \
метод максимального правдоподобия это в поиск максимума лайклихуда

$$q = {\underset {q }{\text{arg\ max} }} \ P_q(X)= {\underset {q }{\text{arg\ max} }} ( P_q(x=x_1)P_q(x=x_2)...P_q(x=x_N) )
$$ 

и нам нужно кинуть монетку хуеву тучу раз чтобы оценить q. \
Теперь отвлечемся от метода максимального правдоподобия и представим ситуацию,\
что у нас есть некоторые представления о том какой должна быть величина $q$, например наша монета на глаз симметрична
и мы считаем что q находится где-то в пределах от 0.3 до 0.7 (навскидку/наотьебись), то есть мы можем даже считать, что q как-то распределена.
Типа q врядли равна числу близкому к 0.1 или 0.9 (ибо монета на глаз ровная), но вполне вероятно равна числу в окрестности 0.5 . 
То есть мы можем задать плотность $p(q)$ которая выглядит как-то так:

![image](https://github.com/savvakonst/temp_docs/assets/48749051/892e532a-8a6f-4de5-bfd9-91cc275657f9)


Воопрос: как нам использовать эту гипотезу про распределение параметра $q$ ?
Дело в том что  лайклихуд $P_q(X)$ - это на самом деле условная вероятность, а именно вероятность наблюдать нашу выборку $X$ \
при условии q ( то есть при условии что наш параметр равен чему-то): 

$$P_q(X) = P(X|q) $$

например вероятность наблюдать нашу выборку при условии что q=0.3 это 

$$P(X|q = 0.3) = P_{0.3}(X)=P_{0.3}(x=x_1)P_{0.3}(x=x_2)...P_{0.3}(x=x_N) =$$

$$=0.3^{x_1} (1-0.3)^{1-x_1} 0.3^{x_2}(1-0.3)^{1-x_2} .. 0.3^{x_N}(1-0.3)^{1-x_N} 
$$

идем дальше используя то, что $P_q(X)$ - условная вероятность, будем использовать теорему байеса, которая выглядит так если B - дискретная :

$$P(B|A) = \frac{P(A|B)P(B)}{ \sum\limits_{i} P(A|B_i)P(B_i)}  $$

или если $b$ непрерывная 

$$ p(b|A) = \frac{P(A|b)p(b)}{ \int\limits P(A|b)p(b)db}   $$


Применительно к нашему случаю получаем:

$$  p(q|X) = \frac{P(X|q)p(q)}{ \int\limits P(X|q)p(q)dq} $$

Что же мы получили в итоге? Что такое есть $p(q|X)$ ?  А это новое распределение параметра $q$ с учетом данных эксперимента X.   

для адекватной монеты p(q|X) будет каким-то таким:

![image](https://github.com/savvakonst/temp_docs/assets/48749051/8b586377-61a5-4f9a-bd48-d27439f2d867)


Мы видим, что плотность сгустилась и теперь мы можем более уверенно утверждать, то что параметр $q$ для нашей модели лежит где-то в пределах от 0.3 до 0.7.
То есть идея в том чтобы не искать конкретное $q$ для нашей модели как с помощью метода максимального провдоподобия, а искать как бы вероятности для $q$ принимать то или иное значение.

P.S. Может появиться закономерный вопрос: ну нашли мы это $p(q|X)$, а как предсказывать то значение $x$? мы же хотим обучить нашу модель??  

а вот как: нам нужно найти вот такую вероятность $P(x|X)$ - вероятность того что x примет какое-то значение при условии чтно нам известны результаты экспериментов. \
Отметим одно свойство нашей модели ибо оно нам понадобится: $P(x|q,X) = P(x|q)$  см.* \
Напишем сначала нашу цель, потом промежуточные выражения (совет: читать вывод справа налево)  

$$ 
P(x|X) = \int p(x,q|X) dq = \int P(x|q,X) p(q|X) dq = \int P(x|q) p(q|X) dq = \int q^x(1-q)^{1-x} p(q|X) dq 
$$ 


#### *если интересно почему P(x|q,X) = P(x|q), смотри далее:
так как $P(x,X|q)=P(x|q)p(X|q)$ то
$$P(x|q,X) = \frac{P(x,q,X)}{p(q,X)} = \frac{P(x,X|q)p(q)}{p(q,X)} = \frac{P(X|q) P(x|q) p(q)}{p(q,X)} = \frac{P(X,q) P(x|q) p(q)}{ p(q) p(q,X)} = P(x|q)  $$
