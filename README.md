# clean-code-javascript

## Table of Contents

1. [შესავალი](#შესავალი)
2. [ცვლადები](#ცვლადები)
3. [ფუნქციები](#ფუნქციები)
4. [ობიექტები და მონაცემთა სტრუქტურები](#ობიექტები-და-მონაცემთა-სტრუქტურები)
5. [კლასები](#კლასები)
6. [SOLID](#solid)
7. [ტესტირება](#ტესტირება)
8. [ასინქრონულობა](#ასინქრონულობა)
9. [Error Handling](#error-handling)
10. [ფორმატირება](#ფორმატირება)
11. [კომენტარები](#კომენტარები)
12. [თარგმანები](#თარგმანები)

## შესავალი

![Humorous image of software quality estimation as a count of how many expletives
you shout when reading code](https://www.osnews.com/images/comics/wtfm.jpg)

სტატიაში აღწერილია კოდის სწორად წერის პრინციპები, რომელიც წარმოადგენს რობერტ ს. მარტინის წიგნის [_Clean Code_](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882),
ადაპტირებულ ვარიანტს JavaScript-ისთვის. სახელმძღვანელოში აღწერილია პრინციპები, თუ როგორ უნდა დაიწეროს კოდი JavaScript-ში, რათა ის იყოს [წაკითხვადი (readable), ხელახლა გამოყენებადი (reusable) და რეფაქტორირებადი (refactorable).](https://github.com/ryanmcdermott/3rs-of-software-architecture)

რა თქმა უნდა სახელმძღვანელოში წარმოდგენილ ყველა პრინციპს მკაცრად ვერ დაიცავთ, თუმცა გახსოვდეთ, რომ ეს გაიდლაინები შემუშავებულია _Clean Code_-ის ავტორთა მრავალწლიანი გამოცდილების გათვალისწინებით.

პროგრამული უზრუნველყოფის შემუშავებას სულ რაღაც 50 წლიანი ისტორია აქვს. ამ თვალსაზრისით ჩვენ ჯერ კიდევ ბევრი გვაქვს სასწავლი. ასე, რომ პროგრამული უზრუნველყოფის არქიტექტურასთან დაკავშირებული წესები ჯერ კიდევ ჩამოყალიბების სტადიაზეა.   
და კიდევ ერთი რამ: ამ გაიდლაინის წაკითხვით ეგრევე არ გახდებით საუკეთესო პროგრამისტი. არ გეგონოთ, რომ კოდის წერისას  შეცდომებს არ დაუშვებთ. :) 


## **ცვლადები**

### გამოიყენეთ გააზრებული და სიტყვიერად გამოთქმადი ცვლადის სახელები

**ცუდია:**

```javascript
const yyyymmdstr = moment().format("YYYY/MM/DD");
```

**კარგია:**

```javascript
const currentDate = moment().format("YYYY/MM/DD");
```

**[⬆ გადასვლა დასაწყისში](#table-of-contents)**

### გამოიყენეთ ერთი ტერმინი ერთი და იგივე ტიპის ცვლადისთვის

**ცუდია:**

```javascript
getUserInfo();
getClientData();
getCustomerRecord();
```

**კარგია:**

```javascript
getUser();
```

**[⬆ გადასვლა დასაწყისში](#table-of-contents)**

### გამოიყენეთ ძებნადი სახელები

პროგრამისტებს იმაზე მეტი კოდის გარჩევა და წაკითხვა უწევთ, ვიდრე ცხოვრებაში დაწერენ. მნიშვნელოვანია, რომ კოდი, რომელსაც ჩვენ ვწერთ, იკითხებოდეს და ცვლადები ადვილად იძებნებოდეს. ცვლადებზე სახელების არ მინიჭებით პრობლემებს ვუქმნით ჩვენი კოდის  მკითხველს. შეეცადეთ ცვლადის სახელები ძებნადი იყოს. ინსტრუმენტები, როგორიცაა 
[buddy.js](https://github.com/danielstjules/buddy.js) და
[ESLint](https://github.com/eslint/eslint/blob/660e0918933e6e7fede26bc675a0763a6b357c94/docs/rules/no-magic-numbers.md)
დაგეხმარებათ უსახელო მუდმივების იდენტიფიცირებაში.

**ცუდია:**

```javascript
// What the heck is 86400000 for?
setTimeout(blastOff, 86400000);
```

**კარგია:**

```javascript
// მათი გამოცხადება, როგორც მთავრული ასოებით შედგენილი კონსტანტების სახელები.
const MILLISECONDS_PER_DAY = 60 * 60 * 24 * 1000; //86400000;

setTimeout(blastOff, MILLISECONDS_PER_DAY);
```

**[⬆ გადასვლა დასაწყისში](#table-of-contents)**

### გამოიყენეთ განმარტებითი ცვლადები

**ცუდია:**

```javascript
const address = "One Infinite Loop, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
saveCityZipCode(
  address.match(cityZipCodeRegex)[1],
  address.match(cityZipCodeRegex)[2]
);
```

**კარგია:**

```javascript
const address = "One Infinite Loop, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
const [_, city, zipCode] = address.match(cityZipCodeRegex) || [];
saveCityZipCode(city, zipCode);
```

**[⬆ გადასვლა დასაწყისში](#table-of-contents)**

### მოერიდეთ ნაგულისხმევ ცვლადებს

ცვლადის ცხადად დასახელება სჯობს ნაგულისხმევ ცვლადს.

**ცუდია:**

```javascript
const locations = ["Austin", "New York", "San Francisco"];
locations.forEach(l => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  // Wait, what is `l` for again?
  dispatch(l);
});
```

**კარგია:**

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

**[⬆ გადასვლა დასაწყისში](#table-of-contents)**

### არ დაამატოთ არასაჭირო კონტექსტი

თუ თქვენი კლასის/ობიექტის სახელი რაღაცას გეუბნებათ, ნუ გაიმეორებთ იგივე დასახელებას  თქვენი ცვლადის სახელშიც.

**ცუდია:**

```javascript
const Car = {
  carMake: "Honda",
  carModel: "Accord",
  carColor: "Blue"
};

function paintCar(car, color) {
  car.carColor = color;
}
```

**კარგია:**

```javascript
const Car = {
  make: "Honda",
  model: "Accord",
  color: "Blue"
};

function paintCar(car, color) {
  car.color = color;
}
```

**[⬆ გადასვლა დასაწყისში](#table-of-contents)**

### გამოიყენეთ დეფაულტ არგუმენტები გამოთვლის მოკლე სქემის ნაცვლად (short circuiting)

დეფაულტ არგუმენტები ხშირად უფრო გასაგებია, ვიდრე short circuiting . გაითვალისწინეთ, რომ თუ მათ იყენებთ, თქვენი ფუნქცია უზრუნველყოფს მხოლოდ დეფაულტ მნიშვნელობებს `undefined` არგუმენტებისთვის. სხვა "falsy" მნიშვნელობები, როგორიცაა `' '`, `" "`, `false`, `null`, `0` და `NaN`, არ შეიცვლება დეფაულტ მნიშვნელობით.

**ცუდია:**

```javascript
function createMicrobrewery(name) {
  const breweryName = name || "Hipster Brew Co.";
  // ...
}
```

**კარგია:**

```javascript
function createMicrobrewery(name = "Hipster Brew Co.") {
  // ...
}
```

**[⬆ გადასვლა დასაწყისში](#table-of-contents)**

## **ფუნქციები**

### ფუნქციის არგუმენტები (იდეალურია 2 ან ნაკლები არგუმენტი)

ძალიან მნიშვნელოვანია, რომ ფუნქციას გადაეცემოდეს მცირე რაოდენობის პარამეტრები, რადგან ეს აადვილებს ფუნქციის ტესტირებას. სამზე მეტი პარამეტრის გადაცემა იწვევს კომბინატორულ აფეთქებას, ამ შემთხვევაში თითოეული არგუმენტისათვის მოგიწევთ უამრავი სხვადასხვა ვარიანტის გატესტვა.

იდეალურია ერთი ან ორი არგუმენტი. სამ არგუმენტს მაქსიმალურად ერიდეთ. თუ ბევრი არგუმენტი გაქვთ, ისინი უნდა გააერთიანოთ. როგორც წესი, თუ თქვენ გაქვთ ორზე მეტი არგუმენტი, ეს იმას ნიშნავს, რომ ფუნქცია ძალიან ბევრის გაკეთებას ცდილობს. სადაც ეს შესაძლებელია, არგუმენტის სახით უნდა გამოიყენოთ  ზედა დონის ობიექტი.

რადგანაც JavaScript–ი გაძლევთ ობიექტების ეგრევე შექმნის (on the fly) საშუალებას, რომელიმე კლასის საფუძვლად გამოყენების გარეშე, ამიტომ არგუმენტად შეგიძლიათ გამოიყენოთ ზედა დონის ობიექტი.

იმისათვის, რომ ცხადად ჩანდეს თუ რა თვისებების მიღებას ელის ფუნქცია, შეგიძლიათ გამოიყენოთ ES2015/ES6 დესტრუქციის სინტაქსი. ამას აქვს რამდენიმე უპირატესობა:



1. როდესაც უყურებთ ფუნქციის სიგნატურას, ნათელია, თუ რა თვისებები გამოიყენება.
2. Destructuring-ი კლონირებას უკეთებს ფუნქციისათვის გადაცემულ არგუმენტ-ობიექტის პრიმიტიულ მნიშვნელობებს. ეს დაგეხმარებათ თავიდან აიცილოთ გვერდითი მოვლენები. შენიშვნა: ობიექტები და მასივები, რომლებიც არგუმენტ-ობიექტიდანაა დესტრუქტურირებული, არ კლონირდებიან.
3. ლინტერებს შეუძლიათ გაგაფრთხილონ გამოუყენებელი თვისებების შესახებ, რაც შეუძლებელი იქნებოდა დესტრუქტურირების გარეშე.



**ცუდია:**

```javascript
function createMenu(title, body, buttonText, cancellable) {
  // ...
}

createMenu("Foo", "Bar", "Baz", true);

```

**კარგია:**

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

**[⬆ გადასვლა დასაწყისში](#table-of-contents)**

### ფუნქციები ერთ რამეს უნდა აკეთებდნენ

ეს არის ყველაზე მნიშვნელოვანი წესი კოდის წერის დროს. როდესაც ფუნქციები ერთზე მეტ რამეს აკეთებენ, რთულდება მათი გაერთიანება,  ტესტირება და ანალიზი. თუ თქვენ შეძელით ფუნქციის იზოლირება ისე, რომ  ის მხოლოდ ერთ მოქმედებას აკეთბს, მაშინ გაგიადველდებათ საჭიროების შემთხვევაში მისი მარტივად გადაკეთება და თქვენი კოდის გარჩევა გაცილებით ადვილი იქნება. იმ შემთხვევაშიც კი, თუ ამ სახელმძღვანელოდან მხოლოდ ამ რჩევას გაითვალისწინებთ, ბევრ დეველოპერს უკან ჩამოიტოვებთ. :)

**ცუდია:**

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

**კარგია:**

```javascript
function emailActiveClients(clients) {
  clients.filter(isActiveClient).forEach(email);
}

function isActiveClient(client) {
  const clientRecord = database.lookup(client);
  return clientRecord.isActive();
}
```

**[⬆ გადასვლა დასაწყისში](#table-of-contents)**

### ფუნქციის სახელმა უნდა გვითხრას თუ რას აკეთებს ის

**ცუდია:**

```javascript
function addToDate(date, month) {
  // ...
}

const date = new Date();

// ფუნქციის დასახელების მიხედვით ძნელია გაარკვიოთ თუ რა ემეტება
addToDate(date, 1);
```

**კარგია:**

```javascript
function addMonthToDate(month, date) {
  // ...
}

const date = new Date();
addMonthToDate(1, date);
```

**[⬆ გადასვლა დასაწყისში](#table-of-contents)**

### ფუნქციას უნდა ჰქონდეს აბსტრაქციის ერთი დონე

როდესაც გაქვთ აბსტრაქციის ერთზე მეტი დონე, როგორც წესი ფუნქცია ზედმეტად  ბევრს აკეთებს. ფუნქციების დაყოფა იძლევა მათი მრავალჯერადი გამოყენებას საშუალებას ასევე უფრო მარტივდება მისი ტესტირება.

**ცუდია:**

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

**კარგია:**

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

**[⬆ გადასვლა დასაწყისში](#table-of-contents)**

### წაშალეთ დუბლირებული კოდი

მაქსიმალურად შეეცადეთ თავიდან აიცილოთ კოდის დუბლირება. დუბლირებული კოდი იმითაა ცუდი, რომ, თუ მასში ლოგიკის გასწორება დაგჭირდათ, მაშინ ამის გაკეთება ბევრგან მოგიწევთ. 

ხშირად იმიტომ გიწევთ კოდის დუბლირება, რომ კოდში გაქვთ ლოგიკურად ერთმანეთის მსგავსი ადგილები. თუმცა მათ შორის არსებობს განსხვავებები, რაც გაიძულებთ დაწეროთ რამოდენიმე ფუნქცია, რომლებიც ბევრ მსგავს ოპერაციას ასრულებს. კოდის დუბლირების თავიდან აცილება  შესაძლებელია ისეთი აბსტრაქციით, რომელიც დუბლირებულ კოდში არსბულ განსხვავბულ ლოგიკას დაამუშავებს მხოლოდ ერთი ფუნქციით / მოდულით / კლასით.

აბსტრაქციის შექმნას გადამწყვეტი მნიშვნელობა აქვს, ამიტომ თქვენ უნდა გაითვალისწინოთ კლასების პროექტირების პრინციპები (SOLID პრინციპები), რომელიც ქვემოთაა მოცემული. ცუდად შედგენილი აბსტრაქციები შეიძლება უარესი იყოს ვიდრე კოდის დუბლირება, ასე რომ ყურადღებით იყავით აბსტრაქციებთან. სხვა სიტყვებით რომ ვთქვათ, თუ შეგიძლიათ კარგი აბსტრაქციის შედგენა, შეადგინეთ! თუ არადა შეეგუეთ იმ აზრს, რომ ერთი პატარა ლოგიკის შეცვლის გამო მოგიწევთ ბევრ ადგილას კოდის შეცვლა. :)

**ცუდია:**

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

**კარგია:**

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

**[⬆ გადასვლა დასაწყისში](#table-of-contents)**

### ობიექტის ველების დეფაულტ მნიშვნელობები დააყენეთ Object.assign-ით

**ცუდია:**

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

**კარგია:**

```javascript
const menuConfig = {
  title: "Order",
  // User did not include 'body' key
  buttonText: "Send",
  cancellable: true
};

function createMenu(config) {
  let finalConfig = Object.assign(
    {
      title: "Foo",
      body: "Bar",
      buttonText: "Baz",
      cancellable: true
    },
    config
  );
  return finalConfig
  // ახლა უკვე config წერია: {title: "Order", body: "Bar", buttonText: "Send", cancellable: true}
  // ...
}

createMenu(menuConfig);
```

**[⬆ გადასვლა დასაწყისში](#table-of-contents)**

### არ გამოიყენოთ დროშები ფუნქციის პარამეტრებად

დროშების გამოყენება იმას მიუთითებს, რომ ეს ფუნქცია ერთზე მეტ რამეს აკეთებს. თუ ლოგიკური პარამეტრის შესაბამისად ის სხვადასხვა კოდს ასრულებს, მაშინ გაყავით თქვენი ფუნქცია.

**ცუდია:**

```javascript
function createFile(name, temp) {
  if (temp) {
    fs.create(`./temp/${name}`);
  } else {
    fs.create(name);
  }
}
```

**კარგია:**

```javascript
function createFile(name) {
  fs.create(name);
}

function createTempFile(name) {
  createFile(`./temp/${name}`);
}
```

**[⬆ გადასვლა დასაწყისში](#table-of-contents)**

### ერიდეთ გვერდით მოვლენებს (ნაწილი 1)

ფუნქცია უნდა იღებდეს პარამეტრების  გარკვეულ მნიშვნელობებს და აბრუნებს სხვა მნიშვნელობებს.  ამის გარდა ის თუ სხვა რამესაც აკეთებს, ესეიგი ფუნქციას აქვს გვერდითი მოვლენები. მაგალითად, გვერდითი მოვლენა შეიძლება იყოს ფაილში ინფორმაციის ჩაწერა, ზოგიერთი გლობალური ცვლადის შეცვლა, ან მთელი ფულის გადატანა უცხო ადამიანის ანგარიშზე.

თუმცა ზოგიერთ შემთხვევაში გვერდით მოვლენებს ვერ აცდებით და დაგჭირდებათ მათი გამოყენება. წინა შემთხვევის მსგავსად თქვენ შეიძლება დაგჭირდეთ გარკვეული ინფორმაციის ჩაწერა ფაილში. არ შექმნათ რამოდენიმე ფუნქცია ან კლასი კონკრეტულ ფაილში ინფორმაციის ჩასაწერად. ამ საქმისთვის უნდა არსებობდეს ერთი სერვისი. მხოლოდ ერთი!

მთავარია, თავიდან ავიცილოთ ისეთი გავრცელებული შეცდომები, როგორებიცაა: ობიექტებს შორის ყოველგვარი სტრუქტურის გარეშე  გაზიარებული state-ები, ასევე ცვალებადი ტიპების მონაცემთა გამოყენება, რომლებსაც შეიძლება ნებისმიერი მნიშვნელობა მიენიჭოს. თუ ამის თავიდან აცილებას შეძლებთ, თქვენ უფრო ბედნიერი იქნებით, ვიდრე პროგრამისტების დიდი უმრავლესობა.


**ცუდია:**

```javascript
// Global variable referenced by following function.
// If we had another function that used this name, now it'd be an array and it could break it.
// Глобальная переменная, на которую ссылается следующая функция. 554 
// Если бы у нас была другая функция, которая использовала бы это имя, теперь это был бы массив, и она могла бы его сломать.
// გლობალური ცვლადი, რომელსაც მიმართავს მისი მომდევნო ფუნცია
// ფუნქცია ცვლადს გარდაქმნის მასივად. სხვა ფუნქცია რომ არსებობდეს, რომელიც იგივე სახელს იყენებს, მაშინ ეს გამოიწვევდა მის გატეხვას 
let name = "Ryan McDermott";

function splitIntoFirstAndLastName() {
  name = name.split(" ");
}

splitIntoFirstAndLastName();

console.log(name); // ['Ryan', 'McDermott'];
```

**კარგია:**

```javascript
function splitIntoFirstAndLastName(name) {
  return name.split(" ");
}

const name = "Ryan McDermott";
const newName = splitIntoFirstAndLastName(name);

console.log(name); // 'Ryan McDermott';
console.log(newName); // ['Ryan', 'McDermott'];
```

**[⬆ გადასვლა დასაწყისში](#table-of-contents)**

### აიცილეთ გვერდითი მოვლენები (ნაწილი 2)

JavaScript-ში ზოგიერთი მნიშვნელობები (values) უცვლელია, ზოგიერთი კი ცვალებადია. ობიექტები და მასივები შედგებიან ცვალებადი მნიშვნელობებისგან, ამიტომ როდესაც ისინი პარამეტრებად გადაეცემიან ფუნქციას, სიფრთხილე უნდა გამოვიჩინოთ. JavaScript ფუნქციას შეუძლია შეცვალოს ობიექტის თვისებები ან შეცვალოს მასივის შემცველობა, რამაც შეიძლება სხვაგან გამოიწვიოს შეცდომები.

დავუშვათ, არებობს ფუნქცია, რომელიც პარამეტრად იღებს მასივს და ის წარმოადგენს კალათაში (`cart`) ჩაგდებული შესაძენი ნივთების სიას. თუ ფუნქცია შეცვლის ამ მასივს - მაგალითად, დაამატებს ახალ შესაძენ  ნივთს - მაშინ ნებისმიერი სხვა ფუნქცია, რომელიც იყენებს იმავე კალათის მასივს, დამოკიდებული იქნება ამ დამატებულ ნივთზე. ეს შეიძლება კარგიც იყოს და ცუდიც. წარმოვიდგინოთ ცუდი სიტუაცია:

მომხმარებელი აწკაპუნებს ღილაკზე „შესყიდვა“, რომელიც იძახებს `purchase` (შესყიდვის) ფუნქციას, ის აყალიბებს მოთხოვნას (request) და კალათის მასივს აგზავნის  სერვერზე. წარმოვიდგინოთ, რომ ქსელის ცუდი კავშირის გამო, `purchase` ფუნქცი იძულებულია ხელახლა გააგზავნოს მოთხოვნა სერვერზე. რა მოხდება, თუ ამ მომენტში მომხმარებელი შეცდომით დააწკაპუნებს ღილაკზე "კალათაში დამატება" იმ ნივთზე, რომლის დამატება კალათაში არ სურს? თუ ასე მოხდება და ამავე დროს სერვერზე რექვესტი წარმატებით გაიგზავნა, მაშინ ეს შესყიდვის ფუნქცია სერვერზე გაგზავნის შემთხვევით დამატებულ ნივთს, რადგანაც კალათის მასივი `addItemToCart` ფუნქციის მიერ შეიცვალა.

კარგი გამოსავალი იქნებოდა `addItemToCart` ფუნქციას ყოველთვის რომ დაეკლონა `cart` –ის მასივი, შემდეგ დაერედაქტირებინა და ეს დარედაქტირებული კლონი დაებრუნებინა. ამით სხვა ფუნქციებს, რომლებიც იყენებენ კალათის მასივს, არ შეეხებოდათ არავითარი ცვლილებები.

ორი გაფრთხილება ამ მიდგომასთან დაკავშირებით:

1.	შეიძლება რეალურად გსურთ გადასაცემი ობიექტის შეცვლა, მაგრამ როდესაც თქვენ მიეჩვევით სუფთა კოდის წერას აღმოაჩენთ, რომ ეს შემთხვევები საკმაოდ იშვიათია. კოდის ლოგიკა შეიძლება ისე გადაკეთდეს, რომ არ ჰქონდეს გვერდითი მოვლენები!

3.	დიდი ობიექტების კლონირება შეიძლება ძალიან რთული აღმოჩნდეს შესრულების თვალსაზრისით. საბედნიეროდ არსებობს 
   [მშვენიერი  ბიბლიოთეკები ](https://facebook.github.io/immutable-js/) რომლებიც საშუალებას გვაძლევს სწრაფად განხორციელდეს ეს მიდგომა, თან მეხსიერებაც ნაკლებად დაიხარჯოს, როგორც ეს იქნებოდა ობიექტებისა და მასივების ხელით კლონირების შემთხვევაში.

**ცუდია:**

```javascript
const addItemToCart = (cart, item) => {
  cart.push({ item, date: Date.now() });
};
```

**კარგია:**

```javascript
const addItemToCart = (cart, item) => {
  return [...cart, { item, date: Date.now() }];
};
```

**[⬆ გადასვლა დასაწყისში](#table-of-contents)**

### არაფერი ჩაწეროთ გლობალურ ობიექტებში

გლობალური ობიექტის დაბინძურება JavaScript-ში ცუდი პრაქტიკაა, რადგან შეიძლება მოხდეს სხვა ბიბლიოთეკასთან კონფლიქტი და თქვენი API-ს მომხმარებელი დიდ გაუგებრობაში აღმოჩნდეს exception–ის მიღების შემთხვევაშიც კი. მოდით განვიხილოთ მაგალითი: რა ვქნათ, თუკი გვინდა განვახორციელოთ გლობალური ობიექტი `Array`–ის გაფართოება (extend), ისე რომ მას ჰქონდეს `diff`  მეთოდი, რომელიც აჩვენებდა განსხვავებას ორ მასივს შორის? `Array.prototype`-სთვის, თქვენ შეგიძლიათ დაწეროთ ახალი მეთოდი, მაგრამ ის შეიძლება კონფლიკტში მოვიდეს სხვა ბიბლიოთეკასთან, რომელიც ცდილობდა იგივეს გაკეთებას. ხომ არ აჯობებდა, რომ ამ სხვა ბიბლიოთეკის `diff`  მეთოდს ეჩვენებინა არა მასივებს შორის განსხვავება, არამედ მასივების პირველ და ბოლო ელემენტებს შორის განსხვავება? აი რატომაა ბევრად უკეთესი გამოვიყენოთ ES2015/ES6 კლასები და უბრალოდ გავაფართოვოთ (extend) გლობალური ობიექტი `array`. 

**ცუდია:**

```javascript
Array.prototype.diff = function diff(comparisonArray) {
  const hash = new Set(comparisonArray);
  return this.filter(elem => !hash.has(elem));
};
```

**კარგია:**

```javascript
class SuperArray extends Array {
  diff(comparisonArray) {
    const hash = new Set(comparisonArray);
    return this.filter(elem => !hash.has(elem));
  }
}
```

**[⬆ გადასვლა დასაწყისში](#table-of-contents)**

### უპირატესობა მიანიჭეთ ფუნქციონალურ პროგრამირებას იმპერატიულ პროგრამირებასთან შედარებით

JavaScript არ არის Haskell–ისნაერი ფუნქციონალური ენა, მაგრამ ის მშვენივრად მეგობრობს ფუნქციონალურობასთანაც. ფუნქციონალურ ენებზე უფრო სუფთა კოდი იწერება და თან ადვილად იტესტება. გამოიყენეთ პროგრამირების ეს სტილის, როცა კი ეს შესაძლებელია.

**ცუდია:**

```javascript
const programmerOutput = [
  {
    name: "Uncle Bobby",
    linesOfCode: 500
  },
  {
    name: "Suzie Q",
    linesOfCode: 1500
  },
  {
    name: "Jimmy Gosling",
    linesOfCode: 150
  },
  {
    name: "Gracie Hopper",
    linesOfCode: 1000
  }
];

let totalOutput = 0;

for (let i = 0; i < programmerOutput.length; i++) {
  totalOutput += programmerOutput[i].linesOfCode;
}
```

**კარგია:**

```javascript
const programmerOutput = [
  {
    name: "Uncle Bobby",
    linesOfCode: 500
  },
  {
    name: "Suzie Q",
    linesOfCode: 1500
  },
  {
    name: "Jimmy Gosling",
    linesOfCode: 150
  },
  {
    name: "Gracie Hopper",
    linesOfCode: 1000
  }
];

const totalOutput = programmerOutput.reduce(
  (totalLines, output) => totalLines + output.linesOfCode,
  0
);
```

**[⬆ გადასვლა დასაწყისში](#table-of-contents)**

### განახორციელეთ პირობების ინკაფსულაცია

**ცუდია:**

```javascript
if (fsm.state === "fetching" && isEmpty(listNode)) {
  // ...
}
```

**კარგია:**

```javascript
function shouldShowSpinner(fsm, listNode) {
  return fsm.state === "fetching" && isEmpty(listNode);
}

if (shouldShowSpinner(fsmInstance, listNodeInstance)) {
  // ...
}
```

**[⬆ გადასვლა დასაწყისში](#table-of-contents)**

### მოერიდეთ უარყოფით პირობებს

**ცუდია:**

```javascript
function isDOMNodeNotPresent(node) {
  // ...
}

if (!isDOMNodeNotPresent(node)) {
  // ...
}
```

**კარგია:**

```javascript
function isDOMNodePresent(node) {
  // ...
}

if (isDOMNodePresent(node)) {
  // ...
}
```

**[⬆ გადასვლა დასაწყისში](#table-of-contents)**

### მოერიდეთ პირობებს

ერთი შეხედვით შეუძლებელია ამ რეკომენდაციის გათვალისწინება. უმეტეს შემთხვევაში პირველი რეაქცია ასეთია: "როგორ უნდა დავწერო კოდი `if`-ის გარეშე ?". გამოსავალი იმაში მდგომარეობს, რომ ხშირ შემთხვევაში შეგიძლიათ გამოიყენოთ პოლიმორფიზმი `if`-ის ჩასანაცვლებლად. როგორც წესი, ჩნდება სხვა კითხვაც: "კარგი, მაგრამ რატომ უნდა გავითვალისწინო ეგ რჩევა?". პასუხი Clean Code-ის ერთ-ერთ პრინციპში შეგიძლიათ ამოიკითხოთ: ფუნქციამ მხოლოდ ერთი რამ უნდა გააკეთოს. თუ თქვენ გაქვთ კლასები და ფუნქციები, რომლებშიც `if` ოპერატორს იყენებთ, თქვენ ამით აღიარებთ, რომ თქვენი ფუნქცია ერთზე მეტ რამეს აკეთებს. დაიმახსოვრე, ფუნქციამ მხოლოდ ერთი რამე უნდა გააკეთოს.

**ცუდია:**

```javascript
class Airplane {
  // ...
  getCruisingAltitude() {
    switch (this.type) {
      case "777":
        return this.getMaxAltitude() - this.getPassengerCount();
      case "Air Force One":
        return this.getMaxAltitude();
      case "Cessna":
        return this.getMaxAltitude() - this.getFuelExpenditure();
    }
  }
}
```

**კარგია:**

```javascript
class Airplane {
  // ...
}

class Boeing777 extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude() - this.getPassengerCount();
  }
}

class AirForceOne extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude();
  }
}

class Cessna extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude() - this.getFuelExpenditure();
  }
}
```

**[⬆ გადასვლა დასაწყისში](#table-of-contents)**

### მოერიდეთ ტიპის შემოწმებას (ნაწილი პირველი)

JavaScript წრმოადგენს არატიპიზირებულ პროგრამირების ენას, ანუ თქვენს ფუნქციებს შეუძლიათ ნებისმიერი ტიპის არგუმენტი მიიღონ. ზოგჯერ ეს თავისუფლება საქმეს გიფუჭებთ და დიდია ტიპების შემოწმების ცდუნება. ამის თავიდან ასაცილებლად მრავალი გზა არსებობს. პირველი არის – თანმიმდევრული API.

**ცუდია:**

```javascript
function travelToTexas(vehicle) {
  if (vehicle instanceof Bicycle) {
    vehicle.pedal(this.currentLocation, new Location("texas"));
  } else if (vehicle instanceof Car) {
    vehicle.drive(this.currentLocation, new Location("texas"));
  }
}
```

**კარგია:**

```javascript
function travelToTexas(vehicle) {
  vehicle.move(this.currentLocation, new Location("texas"));
}
```

**[⬆ გადასვლა დასაწყისში](#table-of-contents)**

### მოერიდეთ ტიპის შემოწმებას (ნაწილი მეორე)

თუ თქვენ მუშაობთ საბაზისო პრიმიტიულ მნიშვნელობებთან, როგორიცაა სტრიქონები, რიცხვები და მასივები და არ შეგიძლიათ გამოიყენოთ პოლიმორფიზმი, მაგრამ მაინც გჭირდებათ ტიპის შემოწმება, ალბათ ვერ აცდებით TypeScript-ის გამოყენებას. ეს არის ნეიტივ JavaScript-ის მშვენიერი ალტერნატივა, რადგან ის გაძლევთ სტატიკური ტიპიზაციის  საშუალებას ნეიტივ JavaScript სინტაქსის გამდიდრების გზით. JavaScript-ში ტიპის ხელით  შემოწმების პრობლემა ის არის, რომ თუ ყველაფერი კარგად გააკეთე, კოდი ზედმეტად დიდი გამოდის და თქვენს მიერ მიღწეული უსაფრთხოება არ აკომპესირებს კოდის გართულებულ  readable-ს.

არ დააჭუჭყიანოთ JavaScript კოდი, დაწერეთ კარგი ტესტები და ხშირად გაუკეთეთ კოდს რევიუ. ან კიდე ყველაფერი აკონტროლეთ TypeScript-ის გამოყენებით (რომელიც, როგორც ვთქვით, ყოველივე ამის შესანიშნავი ალტერნატივაა!).


**ცუდია:**

```javascript
function combine(val1, val2) {
  if (
    (typeof val1 === "number" && typeof val2 === "number") ||
    (typeof val1 === "string" && typeof val2 === "string")
  ) {
    return val1 + val2;
  }

  throw new Error("Must be of type String or Number");
}
```

**კარგია:**

```javascript
function combine(val1, val2) {
  return val1 + val2;
}
```

**[⬆ გადასვლა დასაწყისში](#table-of-contents)**

### ნუ გადაამლაშებთ კოდის ოპტიმიზაციაში

თანამედროვე ბრაუზერები საკმაოდ კარგ ოპტიმიზაციას უკეთებენ კოდს. უმეტეს შემთხვევაში, თქვენს მიერ კოდის ოპტიმიზაციისთვის დახარჯული დრო ტყვილად დაკარგული დროა. [არსებობს კარგი რესურსები]( https://github.com/petkaantonov/bluebird/wiki/Optimization-killers ), რომლებიც გიჩვენებენ სადაა არასაკმარისად ოპტიმიზირებული კოდი. მანამდე გამოიყენეთ ისინი, სანამ სიტუაცია არ გამოსწორდება. 

**ცუდია:**

```javascript
// ძველ ბრაუზერებში არაქეშირებული `list.length` ყოველი იტერაციის დროს მიმდინარეობდა `list.length`–ის ხელახალი გამოთვლა.
// ახალ ბრაუზერებში ეს უკვე ოპტიმიზირებულია
for (let i = 0, len = list.length; i < len; i++) {
  // ...
}
```

**კარგია:**

```javascript
for (let i = 0; i < list.length; i++) {
  // ...
}
```

**[⬆ გადასვლა დასაწყისში](#table-of-contents)**

### წაშალეთ მკვდარი კოდი

მკვდარი კოდი ისეთივე ცუდია, როგორც დუბლირებული კოდი. არ არსებობს მიზეზი, რომ ის კოდში შეინახოთ. თუ კოდი არ გამოიყენება, მოიშორეთ! საჭიროების შემთხვევაში, მისი აღდგენა ყოველთვის შეგიძლიათ ვერსიების ისტორიიდან.

**ცუდია:**

```javascript
function oldRequestModule(url) {
  // ...
}

function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker("apples", req, "www.inventory-awesome.io");
```

**კარგია:**

```javascript
function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker("apples", req, "www.inventory-awesome.io");
```

**[⬆ გადასვლა დასაწყისში](#table-of-contents)**

## **ობიექტები და მონაცემთა სტრუქტურები**

### გამოიყენეთ გეტერები და სეტერები

ობიექტის მონაცემებზე წვდომისთვის ბევრად უკეთესია, გეტერების და სეტერებს გამოყენება ვიდრე ობიექტის თვისებებზე პირდაპირი წვდომაა. რატომ? წარმოგიდგენთ მიზეზების ჩამონათვალს:

- თუ გინდათ იმაზე მეტი გააკეთოთ, ვიდრე უბრალოდ ობიექტის თვისებები მიიღოთ.
- Set-ის შესრულებისას მარტივადაა შესაძლებელი ვალიდაციის დამატება.
- ახორციელებს შიდა წარმოდგენის (internal representation) ინკაფსულაციას.
- Get-ისა და Set-ის შემთხვევაში ადვილია ლოგირება და შეცდომების დამუშავება.
- თქვენ შეგიძლიათ გამოიყენოთ თქვენი ობიექტის თვისებების `lazy load`. ვთქვათ, სერვერიდან მათი მიღებისას.


**ცუდია:**

```javascript
function makeBankAccount() {
  // ...

  return {
    balance: 0
    // ...
  };
}

const account = makeBankAccount();
account.balance = 100;
```

**კარგია:**

```javascript
function makeBankAccount() {
  // this one is private
  let balance = 0;

  // a "getter", made public via the returned object below
  function getBalance() {
    return balance;
  }

  // a "setter", made public via the returned object below
  function setBalance(amount) {
    // ... validate before updating the balance
    balance = amount;
  }

  return {
    // ...
    getBalance,
    setBalance
  };
}

const account = makeBankAccount();
account.setBalance(100);
```

**[⬆ გადასვლა დასაწყისში](#table-of-contents)**

### ობიექტებში შექმენით პრაივატ ველები

ამის მიღწევა შესაძლებელია closures გამოყენებით (ES5 სტანდარტიდან).

**ცუდია:**

```javascript
const Employee = function(name) {
  this.name = name;
};

Employee.prototype.getName = function getName() {
  return this.name;
};

const employee = new Employee("John Doe");
console.log(`Employee name: ${employee.getName()}`); // Employee name: John Doe
delete employee.name;
console.log(`Employee name: ${employee.getName()}`); // Employee name: undefined
```

**კარგია:**

```javascript
function makeEmployee(name) {
  return {
    getName() {
      return name;
    }
  };
}

const employee = makeEmployee("John Doe");
console.log(`Employee name: ${employee.getName()}`); // Employee name: John Doe
delete employee.name;
console.log(`Employee name: ${employee.getName()}`); // Employee name: John Doe
```

**[⬆ გადასვლა დასაწყისში](#table-of-contents)**

## **კლასები**

### უპირატესობა მიანიჭეთ ES2015/ES6 კლასებს ES5 plain ფუნქციებთან შედარებით

ძალიან რთულია წაკითხვადი (readable) კლასის მემკვიდრეობის, კონსტრუქციის და მეთოდების განმარტებების მიღება კლასიკური ES5 კლასებისთვის. თუ თქვენ გჭირდებათ მემკვიდრეობა, მაშინ უპირატესობა მიანიჭეთ ES2015/ES6 კლასებს. თუმცა, უპირატესობა მიანიჭეთ მცირე ფუნქციების გამოყენებას კლასებთან შედარებით, სანამ არ აღმოჩნდებით, რომ გჭირდებათ უფრო დიდი და კომპლექსური ობიექტები.

**ცუდია:**

```javascript
const Animal = function(age) {
  if (!(this instanceof Animal)) {
    throw new Error("Instantiate Animal with `new`");
  }

  this.age = age;
};

Animal.prototype.move = function move() {};

const Mammal = function(age, furColor) {
  if (!(this instanceof Mammal)) {
    throw new Error("Instantiate Mammal with `new`");
  }

  Animal.call(this, age);
  this.furColor = furColor;
};

Mammal.prototype = Object.create(Animal.prototype);
Mammal.prototype.constructor = Mammal;
Mammal.prototype.liveBirth = function liveBirth() {};

const Human = function(age, furColor, languageSpoken) {
  if (!(this instanceof Human)) {
    throw new Error("Instantiate Human with `new`");
  }

  Mammal.call(this, age, furColor);
  this.languageSpoken = languageSpoken;
};

Human.prototype = Object.create(Mammal.prototype);
Human.prototype.constructor = Human;
Human.prototype.speak = function speak() {};
```

**კარგია:**

```javascript
class Animal {
  constructor(age) {
    this.age = age;
  }

  move() {
    /* ... */
  }
}

class Mammal extends Animal {
  constructor(age, furColor) {
    super(age);
    this.furColor = furColor;
  }

  liveBirth() {
    /* ... */
  }
}

class Human extends Mammal {
  constructor(age, furColor, languageSpoken) {
    super(age, furColor);
    this.languageSpoken = languageSpoken;
  }

  speak() {
    /* ... */
  }
}
```

**[⬆ გადასვლა დასაწყისში](#table-of-contents)**

### გამოიყენეთ chaining პატერნის მეთოდი

ეს პატერნი ძალიან სასარგებლოა JavaScript-ში და მას შეხვდებით მრავალ ბიბლიოთეკაში, როგორიცაა jQuery და Lodash. ეს საშუალებას აძლევს თქვენს კოდს იყოს უფრო ექსპრესიული  და კომპაქტური. ამ მიზეზის გამო გირჩევთ გამოიყენოთ chaining პატერნის მეთოდი და ნახავთ რამდენად სუფთა გამოვა თქვენი კოდი. თქვენი კლასის ფუნქციების ბოლოს მარტივად დააბრუნებთ this-ს და შეძლებთ მას დაუკავშიროთ შემდგომი კლასის მეთოდები.

**ცუდია:**

```javascript
class Car {
  constructor(make, model, color) {
    this.make = make;
    this.model = model;
    this.color = color;
  }

  setMake(make) {
    this.make = make;
  }

  setModel(model) {
    this.model = model;
  }

  setColor(color) {
    this.color = color;
  }

  save() {
    console.log(this.make, this.model, this.color);
  }
}

const car = new Car("Ford", "F-150", "red");
car.setColor("pink");
car.save();
```

**კარგია:**

```javascript
class Car {
  constructor(make, model, color) {
    this.make = make;
    this.model = model;
    this.color = color;
  }

  setMake(make) {
    this.make = make;
    // NOTE: Returning this for chaining
    return this;
  }

  setModel(model) {
    this.model = model;
    // NOTE: Returning this for chaining
    return this;
  }

  setColor(color) {
    this.color = color;
    // NOTE: Returning this for chaining
    return this;
  }

  save() {
    console.log(this.make, this.model, this.color);
    // NOTE: Returning this for chaining
    return this;
  }
}

const car = new Car("Ford", "F-150", "red").setColor("pink").save();
```

**[⬆ გადასვლა დასაწყისში](#table-of-contents)**

### მემკვიდრეობასთან შედარებით უპირატესობა მიანიჭეთ კომპოზიციას

„ოთხი ბანდიტის“ (Gang of Four) მიერ დაწერილ წიგნის [_Design Patterns_](https://en.wikipedia.org/wiki/Design_Patterns)-ის მიხედვით მემკვიდრეობასთან შედარებით  უპირატესობა უნდა მივანიჭოთ კომპოზიციას. თუმცა არსებობს მრავალი არგუმენტი მემკვიდრეობის გამყენების სასარგებლოდ, და პირიქითაც ბევრი კარგი არგუმენტი შეიძლება მოვიყვანოთ კომპოზიციის სასარგებლოდ. ამ მსჯელობის მთავარი აზრი ის არის, რომ თუ თქვენ ინსტინქტურად მაინც მემკვიდრეობისკენ იხრებით, ყოველთვის კითხეთ საკუთარ თავს  ხომ არ შეიძლება პრობლემის უკეთ გადაწყვეტა კომპოზიციის გამოყენებით?

შეიძლება იკითხოთ: "როდის უნდა გამოვიყენო მემკვიდრეობა?" ეს დამოკიდებულია თქვენს პრობლემაზე. წარმოგიდგენთ შემთხვევებს როდესაც კომპოზიციის გამოყენებას ჯობია მემკვიდრეობის გამოყენება:

1. თქვენი მემკვიდრეობა წარმოადგენს `"is-a"` ურთიერთობას და არა `"has-a"` ურთიერთობას (Human->Animal vs. User->UserDetails).
2. შეგიძლიათ ხელახლა გამოიყენოთ კოდი base კლასებიდან (ადამიანებს შეუძლიათ გადაადგილება, ისევე როგორც ცხოველებს).
3. როცა გსურთ გლობალური ცვლილებების შეტანა წარმოებულ კლასებში საბაზისო კლასის შეცვლის გზით. (შეცვალეთ ყველა ცხოველის კალორიების ხარჯი, როცა ისისნი  გადაადგილდებიან).


**ცუდია:**

```javascript
class Employee {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  // ...
}

// ცუდია იმიტომ რომ Employee–ს (თანამშრომელს) "აქვს" საგადასახადო მონაცემები
// EmployeeTaxData არ არის Employee ტიპის. 
class EmployeeTaxData extends Employee {
  constructor(ssn, salary) {
    super();
    this.ssn = ssn;
    this.salary = salary;
  }

  // ...
}
```

**კარგია:**

```javascript
class EmployeeTaxData {
  constructor(ssn, salary) {
    this.ssn = ssn;
    this.salary = salary;
  }

  // ...
}

class Employee {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  setTaxData(ssn, salary) {
    this.taxData = new EmployeeTaxData(ssn, salary);
  }
  // ...
}
```

**[⬆ გადასვლა დასაწყისში](#table-of-contents)**

## **SOLID**

### ერთადერთი პასუხისმგებლობის პრინციპი ( SRP - Single Responsibility Principle)

Clean Code-ის რეკომენდაციაა: "არ უნდა არსებობდეს ერთზე მეტი მიზეზი კლასის შესაცვლელად." წარმოიდგინეთ კლასი, რომელიც გადავსბულია ათასნაირი ფუნქციონალით. ეს იმას გავს, რომ სადმე სამოგზაუროდ მიდიოდეთ და ყველა საჭირო ნივთი მხოლოდ ერთ ჩემოდანში ჩატენოთ. პრობლემა ის არის, რომ თქვენი კლასი არ იქნება კონცეპტუალურად ერთგვაროვანი და მისში ცვლილების შეტანის მრავალი მიზეზი  იარსებებს. ძალიან მნიშვნელოვანია ასეთი მიზეზების რაოდენობა მინიმუმამდე დავიყვანოთ. თუ თქვენ ძალიან ბევრ ფუნქციონალს ჩატენით ერთ კლასში და შემდეგ შეეცდებით შეცვალოთ მისი ნაწილი, მაშინ ძალიან რთული იქნება იმის პროგნოზირება, თუ როგორ შეიძლება ამან გავლენა მოახდინოს სისტემის სხვა მოდულებზე.

**ცუდია:**

```javascript
class UserSettings {
  constructor(user) {
    this.user = user;
  }

  changeSettings(settings) {
    if (this.verifyCredentials()) {
      // ...
    }
  }

  verifyCredentials() {
    // ...
  }
}
```

**კარგია:**

```javascript
class UserAuth {
  constructor(user) {
    this.user = user;
  }

  verifyCredentials() {
    // ...
  }
}

class UserSettings {
  constructor(user) {
    this.user = user;
    this.auth = new UserAuth(user);
  }

  changeSettings(settings) {
    if (this.auth.verifyCredentials()) {
      // ...
    }
  }
}
```

**[⬆ გადასვლა დასაწყისში](#table-of-contents)**

### ღია/დახურული პრინციპი (OCP - Open/Closed Principle)

როგორც ბერტრანდ მეიერმა განაცხადა, პროგრამული ენტიტიები (კლასები, მოდულები, ფუნქციები და ა.შ.) უნდა იყოს ღია გაფართოებისთვის, მაგრამ დახურული უნდა იყოს მოდიფიკაციისთვის. რას ნიშნავს ეს პრაქტიკაში? ეს იმას ნიშნავს, რომ თქვენ უნდა მისცეთ მომხმარებლებს ახალი ფუნქციონალობის დამატების საშუალება, არსებული კოდის შეცვლის გარეშე.

**ცუდია:**

```javascript
class AjaxAdapter extends Adapter {
  constructor() {
    super();
    this.name = "ajaxAdapter";
  }
}

class NodeAdapter extends Adapter {
  constructor() {
    super();
    this.name = "nodeAdapter";
  }
}

class HttpRequester {
  constructor(adapter) {
    this.adapter = adapter;
  }

  fetch(url) {
    if (this.adapter.name === "ajaxAdapter") {
      return makeAjaxCall(url).then(response => {
        // transform response and return
      });
    } else if (this.adapter.name === "nodeAdapter") {
      return makeHttpCall(url).then(response => {
        // transform response and return
      });
    }
  }
}

function makeAjaxCall(url) {
  // request and return promise
}

function makeHttpCall(url) {
  // request and return promise
}
```

**კარგია:**

```javascript
class AjaxAdapter extends Adapter {
  constructor() {
    super();
    this.name = "ajaxAdapter";
  }

  request(url) {
    // request and return promise
  }
}

class NodeAdapter extends Adapter {
  constructor() {
    super();
    this.name = "nodeAdapter";
  }

  request(url) {
    // request and return promise
  }
}

class HttpRequester {
  constructor(adapter) {
    this.adapter = adapter;
  }

  fetch(url) {
    return this.adapter.request(url).then(response => {
      // transform response and return
    });
  }
}
```

**[⬆ გადასვლა დასაწყისში](#table-of-contents)**

### ლისკოვის ჩანაცვლების პრინციპი (LSP - Liskov Substitution Principle)

ამ რთული ტერმინის უკან მარტივი კონცეფცია დგას. ფორმალურად ის ასე ჟღერს: "თუ `S` არის `T`-ს ქვეტიპი, მაშინ `T` ტიპის ობიექტები შეიძლება შეიცვალოს `S` ტიპის ობიექტებით (ანუ `S` ტიპის ობიექტებს შეუძლიათ შეცვალონ `T` ტიპის ობიექტები) პროგრამის თვისებებზე მნიშვნელოვანი ზემოქმედების გარეშე ( კორექტულობა, შესრულებადი ტასკი და ა.შ.). მგონი  განმარტება კიდევ უფრო ჩახლართული გამოვიდა.

საუკეთესო ახსნა არის ის, რომ თუ თქვენ გაქვთ მშობელი და შვილობილი კლასები, მაშინ მათი გამოყენებისას ისინი შეიძლება ერთმანეთს ჩაენაცვლონ, არასწორი შედეგების მიღების გარეშე. ასეთი განმარტებაც  შეიძლება დამაბნეველი იყოს.  მოდით შევხედოთ კლასიკურ კვადრატი-მართკუთხედის მაგალითს. მათემატიკურად, კვადრატი არის მართკუთხედი, მაგრამ თუ მათ ურთიერთობას მემკვიდრეობის საშუალებით დაამოდელირებთ  ("წარმოადგენს ნაირსახეობას"), სწრაფად წააწყდებით პრობლემებს.


**ცუდია:**

```javascript
class Rectangle {
  constructor() {
    this.width = 0;
    this.height = 0;
  }

  setColor(color) {
    // ...
  }

  render(area) {
    // ...
  }

  setWidth(width) {
    this.width = width;
  }

  setHeight(height) {
    this.height = height;
  }

  getArea() {
    return this.width * this.height;
  }
}

class Square extends Rectangle {
  setWidth(width) {
    this.width = width;
    this.height = width;
  }

  setHeight(height) {
    this.width = height;
    this.height = height;
  }
}

function renderLargeRectangles(rectangles) {
  rectangles.forEach(rectangle => {
    rectangle.setWidth(4);
    rectangle.setHeight(5);
    const area = rectangle.getArea(); // ცუდია: ვადრატისთვის აბრუნებს 25. უნდა იყოს 20.
    rectangle.render(area);
  });
}

const rectangles = [new Rectangle(), new Rectangle(), new Square()];
renderLargeRectangles(rectangles);
```

**კარგია:**

```javascript
class Shape {
  setColor(color) {
    // ...
  }

  render(area) {
    // ...
  }
}

class Rectangle extends Shape {
  constructor(width, height) {
    super();
    this.width = width;
    this.height = height;
  }

  getArea() {
    return this.width * this.height;
  }
}

class Square extends Shape {
  constructor(length) {
    super();
    this.length = length;
  }

  getArea() {
    return this.length * this.length;
  }
}

function renderLargeShapes(shapes) {
  shapes.forEach(shape => {
    const area = shape.getArea();
    shape.render(area);
  });
}

const shapes = [new Rectangle(4, 5), new Rectangle(4, 5), new Square(5)];
renderLargeShapes(shapes);
```

**[⬆ გადასვლა დასაწყისში](#table-of-contents)**

### ინტერფეისის გაყოფის პრინციპი  (ISP - Interface Segregation Principle)

JavaScript- ს არ აქვს ინტერფეისები, ამიტომ ეს პრინციპი არ გამოიყენება ისე მკაცრად, როგორც სხვა პრინციპები. მიუხედავად ამისა, მნიშვნელოვანია და აქტუალურია მათი გათვალისწინებაც. 

ISP -ს პრინციპი ამბობს, რომ კლიენტები არ უნდა იყვნენ დამოკიდებული იმ ინტერფეისებზე, რომლებსაც ისინი არ იყენებენ. ტიპიზაციის არარსებობის გამო JavaScript- ში ინტერფეისები წარმოადგენენ არაცხად კონტრაქტებს.

კარგ მაგალითად გამოდგება კლასები, რომლებიც ობიექტებისთვის ითვალისწინებენ მრავალნაირ settings. სწორი იქნება, თუ კლიენტს არ მოვთხოვთ ყველა settings-ის დაყენებას, რადგან უმეტეს შემთხვევაში ისინი არც არიან საჭირო. ოპციონლალური settings-ების  შექმნა თვიდან აგვაცილებს ინტერფეისის გაბერვას.


**ცუდია:**

```javascript
class DOMTraverser {
  constructor(settings) {
    this.settings = settings;
    this.setup();
  }

  setup() {
    this.rootNode = this.settings.rootNode;
    this.settings.animationModule.setup();
  }

  traverse() {
    // ...
  }
}

const $ = new DOMTraverser({
  rootNode: document.getElementsByTagName("body"),
  animationModule() {} // უმეტეს შემთხვევაში არ გამოგვადგება ანიმაცია.
  // ...
});
```

**კარგია:**

```javascript
class DOMTraverser {
  constructor(settings) {
    this.settings = settings;
    this.options = settings.options;
    this.setup();
  }

  setup() {
    this.rootNode = this.settings.rootNode;
    this.setupOptions();
  }

  setupOptions() {
    if (this.options.animationModule) {
      // ...
    }
  }

  traverse() {
    // ...
  }
}

const $ = new DOMTraverser({
  rootNode: document.getElementsByTagName("body"),
  options: {
    animationModule() {}
  }
});
```

**[⬆ გადასვლა დასაწყისში](#table-of-contents)**

### დეპენდენსების ინვერსიის პრინციპი (DIP - Dependency Inversion Principle)

ეს პრინციპი აგვარებს ორ მნიშვნელოვან რამეს:

1. ზედა დონის მოდულები არ უნდა იყოს დამოკიდებული ქვედა დონის მოდულებზე. ერთიც და მეორეც აბსტრაქციებზე უნდა იყოს დამოკიდებული.

2. აბსტრაქციები არ უნდა იყოს დამოკიდებული დეტალებზე. არამედ დეტალები უნდა იყოს დამოკიდებული აბსტრაქციებზე.

ერთი შეხედვით ეს რთული ჩანს, მაგრამ თუ თქვენ მუშაობდით Angular.js- თან, გემახსოვრებათ, რომ  ეს პრინციპი რეალიზებული იყო დეპენდენს ინჯექშენის გამოყენებით ( Dependency Injection - DI). მიუხედავად იმისა , რომ DIP და DI არ არის იდენტური ცნებები, DIP იცავს  ზედა დონის მოდულებს ქვედა დონის მოდულების დეტალებისგან და ამას აკეთებს DI- ს მეშვეობით. DIP- ის მათავარი სიკეთე მოდულებს შორის ურთიერთკავშირების შემცირებაა. მოდულების ჩახლართულობა არის ანტიპატერნი, რადგან ეს თქვენი კოდის რეფაქტორირებას საკმაოდ ართულებს.

როგორც ზემოთ აღინიშნა, JavaScript- ს არ აქვს ინტერფეისები, ასე რომ აბსტრაქციები დამოკიდებულია არაცხად კონტრაქტებზე (implicit contracts). ანუ დამოკიდებულია მეთოდებზე და თვისებებზე, რომელსაც ობიექტი/კლასი აძლევს სხვა ობიექტს/კლასს. ქვემოთ მოცემულ მაგალითში, არაცხადი კონტრაქტი (implicit contracts) იმაში მდგომარეობს, რომ InventoryTracker- ის მოთხოვნისთვის ნებისმიერ მოდულს ექნება requestItems მეთოდი.


**ცუდია:**

```javascript
class InventoryRequester {
  constructor() {
    this.REQ_METHODS = ["HTTP"];
  }

  requestItem(item) {
    // ...
  }
}

class InventoryTracker {
  constructor(items) {
    this.items = items;

    // ცუდია: ჩვენ შევქმენით დამოკიდებულება მოთხოვნის კონკრეტული რეალიზაციისათვის.
    // ჩვენ უბრალოდ გვინდა, რომ requestItems დამოკიდებული იყოს მოთხოვნის მეთოდზე :  `request`
    this.requester = new InventoryRequester();
  }

  requestItems() {
    this.items.forEach(item => {
      this.requester.requestItem(item);
    });
  }
}

const inventoryTracker = new InventoryTracker(["apples", "bananas"]);
inventoryTracker.requestItems();
```

**კარგია:**

```javascript
class InventoryTracker {
  constructor(items, requester) {
    this.items = items;
    this.requester = requester;
  }

  requestItems() {
    this.items.forEach(item => {
      this.requester.requestItem(item);
    });
  }
}

class InventoryRequesterV1 {
  constructor() {
    this.REQ_METHODS = ["HTTP"];
  }

  requestItem(item) {
    // ...
  }
}

class InventoryRequesterV2 {
  constructor() {
    this.REQ_METHODS = ["WS"];
  }

  requestItem(item) {
    // ...
  }
}

// ჩვენი დამოკიდებულებების გარედან შექმნისა და დანერგვის გზით ჩვენ შეგვიძლია 
// მოთხოვნის მოდული მარტივად შევცვალოთ ახალი მიდგომით, მაგალითად WebSockets–ით.
const inventoryTracker = new InventoryTracker(
  ["apples", "bananas"],
  new InventoryRequesterV2()
);
inventoryTracker.requestItems();
```

**[⬆ გადასვლა დასაწყისში](#table-of-contents)**

## **ტესტირება**

ტესტირება უფრო მნიშვნელოვანია, ვიდრე დეპლოიმენტი. თუ კოდისთვის ცოტა ტესტი გაქვთ დაწერილი ან ის საერთოდ არ გაქვთ, მაშინ პროდაქშენზე კოდის აწევისას, არ იქნბით დარწმუნებული რომ ყველაფერი სწორად იმუშავებს. თქვენი გუნდის გადასაწყვეტია, საკმარისადაა თუ არა თქვენი კოდი ტესტებით დაფარული. ტესტებით კოდის 100%-ის დაფარვა უზრუნველყოფს თქვენი კოდის მაღალ სანდოობას და თქვენც  გაცილებით მშვიდად იქნებით. გარდა იმისა, რომ უნდა გქონდეთ არჩეული ტესტირების ფრეიმვორკი, ასევე უნდა გამოიყენოთ ტესტებით დაფარვის [კარგი ინსტრუმენტი](https://gotwarlost.github.io/istanbul/).

გამართლება არა აქვს პროექტში ტესტების არარსებობას. JavaScript- სთვის არსებობს ტესტირების [ბევრი კარგი ფრეიმვორკი](https://jstherightway.org/#testing-tools), ასე რომ შეარჩიეთ თქვენთვის მისაღები ფრეიმვორკი და შეეცადეთ დაწეროთ ტესტები ყოველი ახალი ფუნქციისთვის და ახალი მოდულისთვის. კარგია თუ უპირატესობას მიანიჭებთ Test Driven Development ( TDD ). მაგრამ მთავარია დარწმუნდეთ, რომ ყოველი ახალი ფიჩის აწევის ან არსებულის რეფაქტორინგის შემდეგ კოდი საკმარისადაა დაფარული ტესტებით.


### ყოველ ქეისს ერთი ტესტი უნდა შეესაბამებოდეს

**ცუდია:**

```javascript
import assert from "assert";

describe("MomentJS", () => {
  it("handles date boundaries", () => {
    let date;

    date = new MomentJS("1/1/2015");
    date.addDays(30);
    assert.equal("1/31/2015", date);

    date = new MomentJS("2/1/2016");
    date.addDays(28);
    assert.equal("02/29/2016", date);

    date = new MomentJS("2/1/2015");
    date.addDays(28);
    assert.equal("03/01/2015", date);
  });
});
```

**კარგია:**

```javascript
import assert from "assert";

describe("MomentJS", () => {
  it("handles 30-day months", () => {
    const date = new MomentJS("1/1/2015");
    date.addDays(30);
    assert.equal("1/31/2015", date);
  });

  it("handles leap year", () => {
    const date = new MomentJS("2/1/2016");
    date.addDays(28);
    assert.equal("02/29/2016", date);
  });

  it("handles non-leap year", () => {
    const date = new MomentJS("2/1/2015");
    date.addDays(28);
    assert.equal("03/01/2015", date);
  });
});
```

**[⬆ გადასვლა დასაწყისში](#table-of-contents)**

## **ასინქრონულობა**

### გამოიყენეთ პრომისება და არა Callback ფუნქციები

Callback ფუნქცია აუარესებს კოდის კითხვადობას,  იწვევს კოდის ზედმეტ ჩალაგებულობას (nesting).
ES 2015 / ES6 სტანდარტში Promises-ი არის ჩაშენებული გლობალური ტიპი. გამოიყენეთ ისინი!

**ცუდია:**

```javascript
import { get } from "request";
import { writeFile } from "fs";

get(
  "https://en.wikipedia.org/wiki/Robert_Cecil_Martin",
  (requestErr, response, body) => {
    if (requestErr) {
      console.error(requestErr);
    } else {
      writeFile("article.html", body, writeErr => {
        if (writeErr) {
          console.error(writeErr);
        } else {
          console.log("File written");
        }
      });
    }
  }
);
```

**კარგია:**

```javascript
import { get } from "request-promise";
import { writeFile } from "fs-extra";

get("https://en.wikipedia.org/wiki/Robert_Cecil_Martin")
  .then(body => {
    return writeFile("article.html", body);
  })
  .then(() => {
    console.log("File written");
  })
  .catch(err => {
    console.error(err);
  });
```

**[⬆ გადასვლა დასაწყისში](#table-of-contents)**

### Async/Await კიდევ უფრო სუფთაა ვიდრე Promises

Promises არის Callback-ის ძალიან სუფთა ალტერნატივა, მაგრამ ES 2017/ ES 8 სტანდარტში გვაქვს async და await რომლებითაც კოდი კიდევ უფრო სუფთა გამოდის. ძალიან მარტივად შეგიძლიათ დაწეროთ ფუნქცია `async` საკვანძო სიტყვით, რის შემდეგაც შეგიძლიათ იმპერატიულად  დაწეროთ ლოგიკა - then chains-ის გამოყენების გარეშე. თუ ახლავე შეძლებთ ES 2017/ES8 ფუნქციების დანერგვას, გამოიყენეთ `async/wait`!

**ცუდია:**

```javascript
import { get } from "request-promise";
import { writeFile } from "fs-extra";

get("https://en.wikipedia.org/wiki/Robert_Cecil_Martin")
  .then(body => {
    return writeFile("article.html", body);
  })
  .then(() => {
    console.log("File written");
  })
  .catch(err => {
    console.error(err);
  });
```

**კარგია:**

```javascript
import { get } from "request-promise";
import { writeFile } from "fs-extra";

async function getCleanCodeArticle() {
  try {
    const body = await get(
      "https://en.wikipedia.org/wiki/Robert_Cecil_Martin"
    );
    await writeFile("article.html", body);
    console.log("File written");
  } catch (err) {
    console.error(err);
  }
}

getCleanCodeArticle()
```

**[⬆ გადასვლა დასაწყისში](#table-of-contents)**

## **Error Handling**

Thrown errors სჭირო რამეა! ის გულისხმობს, რომ როდესაც თქვენი პროგრამის შესრულებისას რაღაც არასწორედ წარიმართა, მოხდება შეცდომის დაფიქსირება. შეჩერდება ფუნქციის შესრულება, Node-ში შეჩრდება პროცესი და გამოიგზავნება კონსოლის შეტყობინება stack trace-სთან ერთად.

### ნუ დააიგნორებთ აღმოჩენილ შეცდომებს

თუ დააიგნორებთ აღმოჩენილ შეცდომას, მაშინ ვერ  შეძლებთ შეასწოროთ ან რაიმე სახის რეაგირება მოახდინოთ მის დადგომაზე. კონსოლში შეცდომების აღრიცხვა (`console.log`) ბევრს არაფერს გაძლევთ, რადგან ის ხშირად შეიძლება დაიკარგოს კონსოლში გამოტანილი შეტყობინებების ზღვაში. `try/catch` სტრუქტურაში კოდის ნაწილის მოქცევა ნიშნავს, რომ თუ ამ კოდის ნაწილში რამე არასწორად წავიდა, გაწერილი გაქვთ მკაფიო გეგმა ამ შეცდომების ჰენდლინგისათვის.

**ცუდია:**

```javascript
try {
  functionThatMightThrow();
} catch (error) {
  console.log(error);
}
```

**კარგია:**

```javascript
try {
  functionThatMightThrow();
} catch (error) {
  // One option (more noisy than console.log):
  console.error(error);
  // Another option:
  notifyUserOfError(error);
  // Another option:
  reportErrorToService(error);
  // OR do all three!
}
```

### ნუ დააიგნორებთ rejected პრომისები

ისევე როგორც try/catch-ის შემთხვევაში არ უნდა დააიგნოროთ rejected პრომისები.

**ცუდია:**

```javascript
getdata()
  .then(data => {
    functionThatMightThrow(data);
  })
  .catch(error => {
    console.log(error);
  });
```

**კარგია:**

```javascript
getdata()
  .then(data => {
    functionThatMightThrow(data);
  })
  .catch(error => {
    // One option (more noisy than console.log):
    console.error(error);
    // Another option:
    notifyUserOfError(error);
    // Another option:
    reportErrorToService(error);
    // OR do all three!
  });
```

**[⬆ გადასვლა დასაწყისში](#table-of-contents)**

## **ფორმატირება**

ფორმატირება ძალიან სუბიექტური საკითხია. არ არსებობს მკაცრი წესები, როდესაც საქმე ეხება ფორმატირებას. მთავარია, ამაზე კამათში დრო არ დაკარგოთ. არსებობს [უამრავი ინსტრუმენტი](https://standardjs.com/rules.html) ამ პროცესის ავტომატიზაციისთვის. ამოირჩიე ერთი-ერთი მათგანი! 


### იყავით თანმიმდევრული მთავრული ასოების გამოყენებისას

JavaScript არატიპიზირებული ენაა, ამიტომ მთავრულმა ასოებმა გარკვეული ინფორმაცია შეიძლება მოგვცეს ცვლადების, ფუნქციების, კლასების შესახებ. წესები სუბიექტურია, თქვენს გუნდს შეუძლია აირჩიოს ნებისმიერი წესი. მთავარია, რასაც აირჩევთ, იყავით მის მიმართ ერთგული და თანმიმდევრული.

**ცუდია:**

```javascript
const DAYS_IN_WEEK = 7;
const daysInMonth = 30;

const songs = ["Back In Black", "Stairway to Heaven", "Hey Jude"];
const Artists = ["ACDC", "Led Zeppelin", "The Beatles"];

function eraseDatabase() {}
function restore_database() {}

class animal {}
class Alpaca {}
```

**კარგია:**

```javascript
const DAYS_IN_WEEK = 7;
const DAYS_IN_MONTH = 30;

const SONGS = ["Back In Black", "Stairway to Heaven", "Hey Jude"];
const ARTISTS = ["ACDC", "Led Zeppelin", "The Beatles"];

function eraseDatabase() {}
function restoreDatabase() {}

class Animal {}
class Alpaca {}
```

**[⬆ გადასვლა დასაწყისში](#table-of-contents)**

### განათავსეთ ერთმანეთთან ახლოს გამომძახებელი და გამოსაძახებელი ფუნქციები 

თუ ერთი ფუნქცია იძახებს მეორეს, ეს ფუნქციები ვერტიკალურად ჩაწერეთ ერთმანეთის მიყოლებით. იდეალურ შემთხვევაში, გამომძახბელი ფუნქცია ჩაწერეთ გამოსაძახებელი ფუნქციის თავზე. 

**ცუდია:**

```javascript
class PerformanceReview {
  constructor(employee) {
    this.employee = employee;
  }

  lookupPeers() {
    return db.lookup(this.employee, "peers");
  }

  lookupManager() {
    return db.lookup(this.employee, "manager");
  }

  getPeerReviews() {
    const peers = this.lookupPeers();
    // ...
  }

  perfReview() {
    this.getPeerReviews();
    this.getManagerReview();
    this.getSelfReview();
  }

  getManagerReview() {
    const manager = this.lookupManager();
  }

  getSelfReview() {
    // ...
  }
}

const review = new PerformanceReview(employee);
review.perfReview();
```

**კარგია:**

```javascript
class PerformanceReview {
  constructor(employee) {
    this.employee = employee;
  }

  perfReview() {
    this.getPeerReviews();
    this.getManagerReview();
    this.getSelfReview();
  }

  getPeerReviews() {
    const peers = this.lookupPeers();
    // ...
  }

  lookupPeers() {
    return db.lookup(this.employee, "peers");
  }

  getManagerReview() {
    const manager = this.lookupManager();
  }

  lookupManager() {
    return db.lookup(this.employee, "manager");
  }

  getSelfReview() {
    // ...
  }
}

const review = new PerformanceReview(employee);
review.perfReview();
```

**[⬆ გადასვლა დასაწყისში](#table-of-contents)**

## **კომენტარები**

### დააკომენტარეთ მხოლოდ ის რასაც აქვს რთული ბიზნეს ლოგიკა

კომენტარების კეთება აუცილებელი მოთხოვნა არ არის. კარგი კოდი საკუთარ თავს თვით ადოკუმენტირებს.

**ცუდია:**

  // Длина строки
  const length = data.length;

  // Цикл через каждый символ в данных
  for (let i = 0; i < length; i++) {
    // Получаем код символа
    const char = data.charCodeAt(i);
    // Создаем хэш
    hash = ((hash << 5) - hash) + char;
    // Преобразуем в 32-битное целое число



```javascript
function hashIt(data) {
  // ჰეში
  let hash = 0;

  // სტრიქონის სიგრძე
  const length = data.length;

  // ციკლი მონაცემების ყოველი სიმბოლოსათვის
  for (let i = 0; i < length; i++) {
    // იღებს სიმბოლოს კოდს
    const char = data.charCodeAt(i);
    // ვქმნით ჰეშს
    hash = (hash << 5) - hash + char;
    // გარდავქმნით 32–თანრიგიან მთელ რიცხვად
    hash &= hash;
  }
}
```

**კარგია:**

```javascript
function hashIt(data) {
  let hash = 0;
  const length = data.length;

  for (let i = 0; i < length; i++) {
    const char = data.charCodeAt(i);
    hash = (hash << 5) - hash + char;

    // გარდავქმნით 32–თანრიგიან მთელ რიცხვად
    hash &= hash;
  }
}
```

**[⬆ გადასვლა დასაწყისში](#table-of-contents)**

### არ დატოვოთ დაკომენტირებული კოდი

გამოიყენეთ ვერსიების კონტროლის სისტემები. დატოვე ძველი კოდი ისტორიაში.

**ცუდია:**

```javascript
doStuff();
// doOtherStuff();
// doSomeMoreStuff();
// doSoMuchStuff();
```

**კარგია:**

```javascript
doStuff();
```

**[⬆ გადასვლა დასაწყისში](#table-of-contents)**

### ნუ გააკეთებთ ქმედებების კომენტირებას

არ დაგავიწყდეთ ვერსიის კონტროლის სისტემების გამოყენება! არავითარი საჭიროება არ არსებობს შეინახოთ მკვდარი კოდი, კომენტირებული კოდი და განსაკუთრებით არ არის საჭირო ქმედებების კომენტარება. გამოიყენეთ `git log`-ი ისტორიის მისაღებად!

**ცუდია:**

```javascript
/**
 * 2016-12-20: ცაშლილია მონადები, ვერ გავიგე რა იყო ის (RM)
 * 2016-10-01: სპეციალური მონადების წარმატებული გამოყენება (JP)
 * 2016-02-03: ტიპების შემოწმება გაუქმებულია (LI)
 * 2015-03-14: დამატებულია combine ტიპის შემოწმებით (JR)
 */
function combine(a, b) {
  return a + b;
}
```

**კარგია:**

```javascript
function combine(a, b) {
  return a + b;
}
```

**[⬆ გადასვლა დასაწყისში](#table-of-contents)**

### მოერიდეთ პოზიციონირების მარკერებს

ისინი უბრალოდ ზედმეტ ხმაურს იწვევენ კოდში. ფუნქციების და ცვლადების სახელები, სტრიქონების სათანადო შეწევები ფორმატიებასთან ერთად, განსაზღვრავენ თქვენი კოდის ვიზუალურ სტრუქტურას.

**ცუდია:**

```javascript
////////////////////////////////////////////////////////////////////////////////
// ობიექტის შექმნა
////////////////////////////////////////////////////////////////////////////////
$scope.model = {
  menu: "foo",
  nav: "bar"
};

////////////////////////////////////////////////////////////////////////////////
// ექშენის დაყენება
////////////////////////////////////////////////////////////////////////////////
const actions = function() {
  // ...
};
```

**კარგია:**

```javascript
$scope.model = {
  menu: "foo",
  nav: "bar"
};

const actions = function() {
  // ...
};
```

**[⬆ გადასვლა დასაწყისში](#table-of-contents)**

## **თარგმანები**

ეს სახელმძღვანელო ასევე ხელმისაწვდომია სხვა ენებზე:

- ![am](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Armenia.png) **სომხური**: [hanumanum/clean-code-javascript/](https://github.com/hanumanum/clean-code-javascript)
- ![bd](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Bangladesh.png) **ბენგალური(বাংলা)**: [InsomniacSabbir/clean-code-javascript/](https://github.com/InsomniacSabbir/clean-code-javascript/)
- ![br](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **ბარზილური პორტუგალიური**: [fesnt/clean-code-javascript](https://github.com/fesnt/clean-code-javascript)
- ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **მარტივი ჩინური**:
  - [alivebao/clean-code-js](https://github.com/alivebao/clean-code-js)
  - [beginor/clean-code-javascript](https://github.com/beginor/clean-code-javascript)
- ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Spain.png) **ესპანური**: [tureey/clean-code-javascript](https://github.com/tureey/clean-code-javascript)
- ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Uruguay.png) **ესპანური**: [andersontr15/clean-code-javascript](https://github.com/andersontr15/clean-code-javascript-es)
- ![en](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags-iso/shiny/24/US.png) **ინგლისური**: [ryanmcdermott/clean-code-javascript](https://github.com/ryanmcdermott/clean-code-javascript)
- ![tw](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Taiwan.png) **ტრადიციული ჩინური**: [AllJointTW/clean-code-javascript](https://github.com/AllJointTW/clean-code-javascript)
- ![fr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/France.png) **ფრანგული**: [GavBaros/clean-code-javascript-fr](https://github.com/GavBaros/clean-code-javascript-fr)
- ![ge](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Georgia.png) **ქართული**: [gochagocha/clean-code-javascript](https://github.com/gochagocha/clean-code-javascript.git)
- ![de](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Germany.png) **გერმანული**: [marcbruederlin/clean-code-javascript](https://github.com/marcbruederlin/clean-code-javascript)
- ![id](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Indonesia.png) **ინდონეზიური**: [andirkh/clean-code-javascript/](https://github.com/andirkh/clean-code-javascript/)
- ![it](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Italy.png) **იტალიური**: [frappacchio/clean-code-javascript/](https://github.com/frappacchio/clean-code-javascript/)
- ![ja](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Japan.png) **იაპონური**: [mitsuruog/clean-code-javascript/](https://github.com/mitsuruog/clean-code-javascript/)
- ![kr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **კორეული**: [qkraudghgh/clean-code-javascript-ko](https://github.com/qkraudghgh/clean-code-javascript-ko)
- ![pl](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Poland.png) **პოლონური**: [greg-dev/clean-code-javascript-pl](https://github.com/greg-dev/clean-code-javascript-pl)
- ![ru](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Russia.png) **რუსული**:
  - [BoryaMogila/clean-code-javascript-ru/](https://github.com/BoryaMogila/clean-code-javascript-ru/)
  - [maksugr/clean-code-javascript](https://github.com/maksugr/clean-code-javascript)
- ![rs](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Serbia.png) **სერბული**: [doskovicmilos/clean-code-javascript/](https://github.com/doskovicmilos/clean-code-javascript)
- ![tr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Turkey.png) **თურქული**: [bsonmez/clean-code-javascript](https://github.com/bsonmez/clean-code-javascript/tree/turkish-translation)
- ![ua](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Ukraine.png) **უკრაინული**: [mindfr1k/clean-code-javascript-ua](https://github.com/mindfr1k/clean-code-javascript-ua)
- ![vi](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Vietnam.png) **ვიეტნამური**: [hienvd/clean-code-javascript/](https://github.com/hienvd/clean-code-javascript/)

**[⬆ გადასვლა დასაწყისშიგადასვლა დასაწყისშიგადასვლა დასაწყისში](#table-of-contents)**
