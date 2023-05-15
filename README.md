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


### Eyni növ dəyişən üçün eyni söz istifadə edin

**Pis:**

```javascript
getUserInfo();
getClientData();
getCustomerRecord();
```

**Yaxşı:**

```javascript
getUser();
```

**[⬆ Mündəricat](#Mündəricat)**

### Rahat axtarıla bilən adlardan istifadə edin

Daha çox oxuyuruq nəinki yazırıq. Rahat oxunan kod yazmaq önəmlidir. Dəyişənləri mənalı və başa düşülən _adlandırmayaraq_, kodu oxuyanın işini çətinə salırıq.
Adlar axtarıla bilən olmalıdır. [buddy.js](https://github.com/danielstjules/buddy.js) və
[ESLint](https://github.com/eslint/eslint/blob/660e0918933e6e7fede26bc675a0763a6b357c94/docs/rules/no-magic-numbers.md) kimi alətlər sizə faydalı ola bilər.

**Pis:**

```javascript
// 86400000 nədir ?
setTimeout(blastOff, 86400000);
```

**Yaxşı:**

```javascript
// Bunları büyük həriflə adlandırılmış sabitlər olaraq təyin edin.
const MILLISECONDS_IN_A_DAY = 86_400_000;

setTimeout(blastOff, MILLISECONDS_IN_A_DAY);
```

**[⬆ Mündəricat](#Mündəricat)**

### Özünü izah edən dəyişənlərdən istifadə edin

**Pis:**

```javascript
const address = "One Infinite Loop, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
saveCityZipCode(
  address.match(cityZipCodeRegex)[1],
  address.match(cityZipCodeRegex)[2]
);
```

**Yaxşı:**

```javascript
const address = "One Infinite Loop, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
const [_, city, zipCode] = address.match(cityZipCodeRegex) || [];
saveCityZipCode(city, zipCode);
```

**[⬆ Mündəricat](#Mündəricat)**

### Başınızda planlaşdırmayın
Örtülü bazar, dostluğu pozar.
İşarə verməyin, aydın izah edin.

**Pis:**

```javascript
const locations = ["Austin", "New York", "San Francisco"];
locations.forEach(l => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  // Dayan görək, 'l' nə idi?
  dispatch(l);
});
```

**Yaxşı:**

```javascript
const locations = ["Austin", "New York", "San Francisco"];
locations.forEach(location => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  dispatch(location);
});
```

**[⬆ Mündəricat](#Mündəricat)**

### Lazımsız kontekst əlavə etməyin

Sinif / obyekt adı sizə bir şey işarə edirsə, bunu dəyişən adında təkrarlamayın.

**Pis:**

```javascript
const Car = {
  carMake: "Honda",
  carModel: "Accord",
  carColor: "Blue"
};

function paintCar(car) {
  car.carColor = "Red";
}
```

**Yaxşı:**

```javascript
const Car = {
  make: "Honda",
  model: "Accord",
  color: "Blue"
};

function paintCar(car) {
  car.color = "Red";
}
```

**[⬆ Mündəricat](#Mündəricat)**

### Qısa qapanma və ya şərti parametrlər əvəzinə standart parametrlərdən istifadə edin

Standart parametrlər qısa dövrədən daha təmizdir. Nəzərə alın ki, onlardan istifadə etsəniz, funksiyanız yalnız _(undefined)_ arqumentlər üçün standart dəyərlər təmin edəcək. `''`, `""`, `false`, `null`, `0` və `NaN` kimi digər "saxta deyilə biləcək" dəyərlər standart dəyərlə əvəz edilməyəcək.

**Pis:**

```javascript
function createMicrobrewery(name) {
  const breweryName = name || "Hipster Brew Co.";
  // ...
}
```

**Yaxşı:**

```javascript
function createMicrobrewery(name = "Hipster Brew Co.") {
  // ...
}
```

**[⬆ Mündəricat](#Mündəricat)**

## **Funksiyalar**

### Funksiya arqumentləri (ideal olaraq 2 və ya daha az)

Funksiya parametrlərinin miqdarının məhdudlaşdırılması olduqca vacibdir, çünki bu, funksiyanızı test etməyi asanlaşdırır. Üçdən çox olması kombinator şişməyə gətirib çıxarır ki, burada hər bir ayrı arqumentlə çoxlu fərqli testlər etməli olursan.

İkidən çox arqument varsa,yaxşı olardı ki onları bir obyektə yığıb obyekti arqument olaraq yollayaq.

Funksiyanın hansı parametrləri gözlədiyini aydınlaşdırmaq üçün  ES2015 / ES6 destruktrizasiya sintaksisi istifadə edə bilərsiz. Faydaları bunlardır:

1. Funksiyaya baxan kimi nə xüsusiyyəti gözlədiyini anlamaq olur..
2. Adlandırılmış parametrləri simulyasiya etmək üçün istifadə edilə bilər.
3. Destrukturizasiya həmçinin funksiyaya ötürülən arqument obyektinin müəyyən edilmiş primitiv qiymətlərini klonlaşdırır. Bu, yan təsirlərin qarşısını almağa kömək edə bilər. Qeyd: destruktrizasiya zamanı obyektlər və massivlər Klonlaşdırılmır!
4. Linterlər sizi istifadə olunmamış xassələr barədə xəbərdar edə bilər ki, bu da destruktrizasiyasız qeyri-mümkün olardı.

**Pis:**

```javascript
function createMenu(title, body, buttonText, cancellable) {
  // ...
}

createMenu("Foo", "Bar", "Baz", true);

```

**Yaxşı:**

```javascript
function createMenu({ title, body, buttonText, cancellable }) {
  // ...
}

createMenu({
  title: "Foo",
  body: "Bar",
  buttonText: "Baz",
  cancellable: true
});
```

**[⬆ Mündəricat](#Mündəricat)**

### Bir funksiya sadəcə bir iş görməlidir

Bu proqram mühəndisliyində ən vacib qaydadır. Funksiyalar birdən çox şeyi yerinə yetirdikdə, onları tərtib etmək, sınamaq  daha çətindir. Bir funksiyanı yalnız bir işi görməyə məcbur etdiyiniz zaman, onu asanlıqla refaktor etmək olar və kodunuz daha təmiz oxunar. Bu bələdçidən başqa heç nə götürməsəniz belə, tək bunu yerinə yetirməklə bir çox tərtibatçıları qabaqlayacaqsınız

**Pis:**

```javascript
function emailClients(clients) {
  clients.forEach(client => {
    const clientRecord = database.lookup(client);
    if (clientRecord.isActive()) {
      email(client);
    }
  });
}
```

**Yaxşı:**

```javascript
function emailActiveClients(clients) {
  clients.filter(isActiveClient).forEach(email);
}

function isActiveClient(client) {
  const clientRecord = database.lookup(client);
  return clientRecord.isActive();
}
```

**[⬆ Mündəricat](#Mündəricat)**

### Funksiayanın adı gördüyü işi ehtiva etməlidir

**Pis:**

```javascript
function addToDate(date, month) {
  // ...
}

const date = new Date();

// Funksiya adı onun hansı parametrləri aldığını demir
addToDate(date, 1);
```

**Yaxşı:**

```javascript
function addMonthToDate(month, date) {
  // ...
}

const date = new Date();
addMonthToDate(1, date);
```

**[⬆ Mündəricat](#Mündəricat)**

### Funksiyalarda  sadəcə bir səviyyə abstraksiya olmalıdır.

Birdən çox abstraksiya olduqda funksiyanız yüklənir,çox iş yerinə yetirir. Mürəkkəb funksiyaların kiçik hissələrə ayrılması oxunaqlığı artırır, test olunmasını asanlaşdırır.

**Pis:**

```javascript
function parseBetterJSAlternative(code) {
  const REGEXES = [
    // ...
  ];

  const statements = code.split(" ");
  const tokens = [];
  REGEXES.forEach(REGEX => {
    statements.forEach(statement => {
      // ...
    });
  });

  const ast = [];
  tokens.forEach(token => {
    // lex...
  });

  ast.forEach(node => {
    // parse...
  });
}
```

**Yaxşı:**

```javascript
function parseBetterJSAlternative(code) {
  const tokens = tokenize(code);
  const syntaxTree = parse(tokens);
  syntaxTree.forEach(node => {
    // parse...
  });
}

function tokenize(code) {
  const REGEXES = [
    // ...
  ];

  const statements = code.split(" ");
  const tokens = [];
  REGEXES.forEach(REGEX => {
    statements.forEach(statement => {
      tokens.push(/* ... */);
    });
  });

  return tokens;
}

function parse(tokens) {
  const syntaxTree = [];
  tokens.forEach(token => {
    syntaxTree.push(/* ... */);
  });

  return syntaxTree;
}
```

**[⬆ Mündəricat](#Mündəricat)**

### Təkrar kodu sil

Bacardığımız qədər dublikat kodlardan qaçmağa çalışaq. Dublikat kod pisdir, çünki bu o deməkdir ki, məntiqi dəyişikliklər etmək lazım olduqda birdən çox yeri dəyişməli olacaqsınız.

Təsəvvür edin ki, bir restoran işlədirsiniz və stokları izləyirsiniz: bütün pomidorlarınız, soğanlarınız, sarımsaqlarınız, ədviyyatlarınız və s. Bu məlumatı saxladığınız bir çox siyahılarınız varsa, bir dəyişiklik olduqda bütün siyahılarda o dəyişikliyi etməli olacaqsınız. Yalnız bir siyahınız varsa, yeniləmək üçün yalnız bir yer var!

Çox vaxt dublikat kodlar yazırsız, çünki sizin iki və ya daha çox bir  fərqli funksiyalarınız olur, amma ortaq cəhətləri olur.Dublikat kodun silinməsi yalnız bir funksiya/modul/sinif ilə bu müxtəlif şeylər dəstini idarə edə biləcək bir abstraksiya yaratmaq deməkdir.

Abstraksiyanı düzgün əldə etmək çox vacibdir, buna görə də siz Dərslər bölməsində verilmiş _SOLID_ prinsiplərinə əməl etməlisiniz. Pis abstraksiyalar dublikat koddan daha pis ola bilər, ona görə də diqqətli olun! Bütün bunlardan sonra yaxşı bir abstraksiya edə bilirsinizsə, bunu edin! Özünüzü təkrarlamayın, əks halda bir şeyi dəyişdirmək istədiyiniz zaman birdən çox yeri yeniləyəcəksiniz.

**Pis:**

```javascript
function showDeveloperList(developers) {
  developers.forEach(developer => {
    const expectedSalary = developer.calculateExpectedSalary();
    const experience = developer.getExperience();
    const githubLink = developer.getGithubLink();
    const data = {
      expectedSalary,
      experience,
      githubLink
    };

    render(data);
  });
}

function showManagerList(managers) {
  managers.forEach(manager => {
    const expectedSalary = manager.calculateExpectedSalary();
    const experience = manager.getExperience();
    const portfolio = manager.getMBAProjects();
    const data = {
      expectedSalary,
      experience,
      portfolio
    };

    render(data);
  });
}
```

**Yaxşı:**

```javascript
function showEmployeeList(employees) {
  employees.forEach(employee => {
    const expectedSalary = employee.calculateExpectedSalary();
    const experience = employee.getExperience();

    const data = {
      expectedSalary,
      experience
    };

    switch (employee.type) {
      case "manager":
        data.portfolio = employee.getMBAProjects();
        break;
      case "developer":
        data.githubLink = employee.getGithubLink();
        break;
    }

    render(data);
  });
}
```

**[⬆ Mündəricat](#Mündəricat)**

### Object.assign ilə standart obyektləri təyin edin

**Pis:**

```javascript
const menuConfig = {
  title: null,
  body: "Bar",
  buttonText: null,
  cancellable: true
};

function createMenu(config) {
  config.title = config.title || "Foo";
  config.body = config.body || "Bar";
  config.buttonText = config.buttonText || "Baz";
  config.cancellable =
    config.cancellable !== undefined ? config.cancellable : true;
}

createMenu(menuConfig);
```

**Yaxşı:**

```javascript
const menuConfig = {
  title: "Order",
  // istifadəçi 'body' əlavə etmədi
  buttonText: "Send",
  cancellable: true
};

function createMenu(config) {
  config = Object.assign(
    {
      title: "Foo",
      body: "Bar",
      buttonText: "Baz",
      cancellable: true
    },
    config
  );

  // config çıxtısı indi : {title: "Order", body: "Bar", buttonText: "Send", cancellable: true}
  // ...
}

createMenu(menuConfig);
```

**[⬆ Mündəricat](#Mündəricat)**

### Funksiya parametrlərində işarələr (flag) istifadə etmə

İşarələr funksiyanın birdən çox iş görməyindən xəbər verir. Biz dedik ki funksiyalar sadəcə bir iş görməlidir.Əgər aşağıdakı kimi Məntiqi operatorlardan istifadə etdiyiniz bir funksiyanız olarsa onu parçalarına ayırın.

**Pis:**

```javascript
function createFile(name, temp) {
  if (temp) {
    fs.create(`./temp/${name}`);
  } else {
    fs.create(name);
  }
}
```

**Yaxşı:**

```javascript
function createFile(name) {
  fs.create(name);
}

function createTempFile(name) {
  createFile(`./temp/${name}`);
}
```

**[⬆ Mündəricat](#Mündəricat)**