# clean-code-javascript

## Mündəricat

1. [Giriş](#Giriş)
2. [Dəyişənlər](#Dəyişənlər)
3. [Funksiyalar](#Funksiyalar)
4. [Obyektlər və Məlumat strukturları](#obyektlər-və-məlumat-strukturları)
5. [Siniflər (Class)](#siniflər)
6. [SOLID](#solid)
7. [Test](#test)
8. [Paralellik](#Paralellik)
9. [Xətalar](#Xətalar)
10. [Formatlaşdırma](#Formatlaşdırma)
11. [Şərhlər](#Şərhlər)
12. [Tərcümələr](#Tərcümələr)

## Giriş

![Kodu oxuyarkən neçə dəfə səhvlə qarşılaşdığınızın kodun keyfiyyətinin təxmininin göstərilməsi](https://www.osnews.com/images/comics/wtfm.jpg)

Robert C. Martin'nın kitabından [_Clean Code_](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882)  proqram təminatı mühəndisliyi prinsiplərinin, JavaScript üçün hazırlanmış vəziyyəti. Bu bir tərz bələdçisi deyil. JavaScript'də [oxunabilən, yenidən istifadə edilə bilən və yenidən modifikasiya edilə bilən](https://github.com/ryanmcdermott/3rs-of-software-architecture) proqramlar hazırlamağa istiqamətlənmiş bir bələdçidir.

Bu təlimatlar, _Clean Code_  yaradıcıları tərəfindən uzun illər boyunca ərsəyə gətirilmiş kollektiv təcrübə əsasında hazırlanmışdır. Təmiz Kod prinsipləri hər hansı bir proqramlaşdırma layihəsində tətbiq edilə bilər, lakin bəzi prinsiplər daha az əhəmiyyətli olaraq qiymətləndirilə bilər. Bunlar sadəcə köməkçi təlimatlardır.

Proqram mühəndisliyi sahəsində uzun illərə dayanan təcrübəmizin olduğunu, lakin hələ də çox şey öyrənməyimizə ehtiyac olduğunu anlayırıq. Proqram arxitekturası vaxt keçdikcə daha da kompleksləşir və bu kompleksliyə uyğun olaraq, daha çətin qaydalar yarana bilər. Bu təlimatlar,  JavaScript kodunun keyfiyyətini qiymətləndirmək üçün xidmət edir.

Daha bir şey: bunları bilmək sizi dərhal daha yaxşı bir proqram tərtibatçısı etməyəcək və uzun illər onlarla işləmək heç vaxt səhv etməyəcəksiniz demək deyil. Hər bir kod parçası əvvəlcə xam olur. Siz, yoldaşlarınızla birlikdə nəzərdən keçirərkən qüsurları aradan qaldırır və onu formaya salırsız. Kodlarınızı daha yaxşı incələyin!

## **Dəyişənlər**

### Mənalı və rahat tələffüz olunan dəyişən adlardan istifadə edin

**Pis:**

```javascript
const yyyymmdstr = moment().format("YYYY/MM/DD");
```

**Yaxşı:**

```javascript
const currentDate = moment().format("YYYY/MM/DD");
```

**[⬆ Mündəricat](#Mündəricat)**

