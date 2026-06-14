# Day 13: Javascript cơ bản — Biến, Kiểu dữ liệu, Toán tử

## Javascript: Ngôn ngữ lập trình web

### Javascript là gì?

- Javascript (JS) là ngôn ngữ lập trình kịch bản, thông dịch, chạy trên trình duyệt và cả server (Node.js).
- Vai trò: làm cho website có hành vi — xử lý sự kiện, thay đổi nội dung, gọi API…
- HTML là cấu trúc, CSS là trình bày, JS là hành vi.

```
HTML -> Nội dung / Cấu trúc
CSS  -> Trang trí / Bố cục
JS   -> Tương tác / Xử lý
```

### Một số đặc điểm

| Đặc điểm        | Giải thích ngắn                                        |
| --------------- | ------------------------------------------------------ |
| Thông dịch      | Không cần biên dịch — trình duyệt đọc và chạy luôn     |
| Động (dynamic)  | Kiểu dữ liệu tự động suy luận, không cần khai báo kiểu |
| Đa năng         | Frontend + Backend + Mobile + Desktop                  |
| Hướng đối tượng | Hỗ trợ prototype, class, object literal                |
| Event-driven    | Phản ứng khi user click, gõ phím, load trang…          |

### Chạy thử Javascript

Mở DevTools (F12 / Cmd+Option+I) -> tab Console -> gõ:

```js
console.log("Hello, world!");
alert("Chào bạn!");
```

## 3 cách tích hợp JS vào web

### 1. Inline — Viết trực tiếp trong thẻ HTML

Dùng thuộc tính sự kiện như `onclick`, `onmouseover`…

```html
<button onclick="alert('Bạn vừa click!')">Nhấn tôi</button>
```

Nhược điểm: khó bảo trì, trộn lẫn HTML và JS — ít dùng trong thực tế.

### 2. Internal — Thẻ `<script>` trong file HTML

Viết JS trong thẻ `<script>` ở `<head>` hoặc cuối `<body>`.

```html
<!DOCTYPE html>
<html lang="vi">
  <head>
    <meta charset="UTF-8" />
    <title>Internal JS</title>
  </head>
  <body>
    <h1>Trang web</h1>

    <script>
      console.log("Internal JS chạy!");
      const name = prompt("Tên bạn là gì?");
      document.querySelector("h1").textContent = "Chào " + name;
    </script>
  </body>
</html>
```

Lưu ý vị trí: để `<script>` ở cuối `body` để đảm bảo DOM đã load trước khi JS chạy, hoặc dùng thuộc tính `defer`.

### 3. External — File `.js` riêng

Tạo file `script.js`, sau đó nhúng vào HTML:

```html
<script src="script.js"></script>
```

script.js:

```js
console.log("External JS — tách biệt hoàn toàn!");
```

#### So sánh ba cách

| Cách     | Ưu điểm                                | Nhược điểm                |
| -------- | -------------------------------------- | ------------------------- |
| Inline   | Nhanh, không cần file phụ              | Khó đọc, khó bảo trì      |
| Internal | Gọn trong một file HTML                | Không tái dùng được       |
| External | Tách biệt, tái dùng, cache bởi browser | Phải load thêm file `.js` |

> Thực tế:

- Luôn dùng External để tách biệt HTML và JS, dễ bảo trì, tái sử dụng.
- Dùng Internal khi script ngắn, chỉ phục vụ cho một trang cụ thể, cần chạy ngay ở đầu trang, giảm số request.
- Dùng Inline tương tụ Internal.

### Thuộc tính `defer` và `async`

Khi dùng `<script src="...">` ở `<head>`, trình duyệt phải tải và chạy JS xong mới render tiếp — gây chậm.

| Thuộc tính | Hành vi                                                          |
| ---------- | ---------------------------------------------------------------- |
| (không)    | Tải và chạy ngay khi gặp, chặn parsing HTML trong suốt quá trình |
| `defer`    | Tải song song với HTML, chạy sau khi HTML parse xong             |
| `async`    | Tải xong lúc nào thì chạy lúc đó, khi chạy thì chặn parsing HTML |

> Thực tế:

- Để ở cuối `body` thì không cần `defer`/`async`, đôi khi gây lỗi hiệu năng.
- Dùng `defer` cho tính năng giao diện, tương tác với DOM.
- Dùng `async` cho script không phụ thuộc vào DOM.

## Biến: var, let, const và scope

### Biến là gì?

Biến là tên tham chiếu đến một vùng nhớ lưu trữ dữ liệu.

```js
let message = "Xin chào";
console.log(message); // "Xin chào"

message = "Hello"; // gán lại giá trị
console.log(message); // "Hello"
```

### Khai báo biến

JS cung cấp ba từ khóa: `var`, `let`, `const`.

| Từ khóa | Có thể gán lại? | Scope (phạm vi)  | Hoisting?                             |
| ------- | --------------- | ---------------- | ------------------------------------- |
| `var`   | ✅ Có           | Function scope   | ✅ Có (được hoisting nhưng undefined) |
| `let`   | ✅ Có           | Block scope `{}` | ✅ Có (TDZ)                           |
| `const` | ❌ Không        | Block scope `{}` | ✅ Có (TDZ)                           |

#### `let` — khai báo có thể gán lại, block scope

```js
let age = 25;
age = 26; // ✅ gán lại được

if (true) {
  let x = 10;
  console.log(x); // 10
}
// console.log(x); // ❌ ReferenceError — x không tồn tại ngoài block
```

#### `const` — hằng số, không thể gán lại, block scope

```js
const PI = 3.14;
// PI = 3.1416; // ❌ TypeError — không gán lại được

const user = { name: "An" };
user.name = "Bình"; // ✅ được — vì const chỉ giữ tham chiếu, object vẫn thay đổi được
// user = {};        // ❌ không gán lại biến được
```

> `const` không có nghĩa là giá trị bất biến, chỉ có nghĩa là tham chiếu không đổi.

#### `var` — cách cũ (tránh dùng)

```js
var name = "An";
name = "Bình"; // ✅ gán lại

if (true) {
  var z = 999;
}
console.log(z); // 999 — var "lọt" ra ngoài block!
```

`var` có function scope, không có block scope, và dễ gây lỗi.

> Thực tế:

- Chỉ dùng `var` khi cần hỗ trợ trình duyệt cũ (IE11 trở về trước) hoặc gặp dự án cũ đã dùng `var`.
- Luôn dùng `const` nếu có thể.
- Nếu không dùng được `const` (ví dụ cần gán lại), thì dùng `let`.

### Scope (phạm vi)

Scope là vùng mã nguồn nơi biến có hiệu lực.

```js
// Global scope
let globalVar = "Tôi ở global";

function demo() {
  // Function scope
  var funcVar = "Tôi trong function";

  if (true) {
    // Block scope
    let blockVar = "Tôi trong block";
    console.log(globalVar); // ✅
    console.log(funcVar); // ✅
    console.log(blockVar); // ✅
  }

  console.log(globalVar); // ✅
  // console.log(blockVar);  // ❌ — blockVar chỉ trong block
}

demo();
```

### Hoisting và TDZ (Temporal Dead Zone)

- Hoisting: khai báo `var`, `let`, `const`, `function` được "kéo lên" đầu scope.
- `var` được hoisting và khởi tạo `undefined` — có thể truy cập trước khi khai báo.
- `let`/`const` được hoisting nhưng không được khởi tạo — truy cập trước dòng khai báo sẽ gặp TDZ -> `ReferenceError`.

```js
console.log(a); // undefined (var được hoisting + khởi tạo undefined)
var a = 5;

// console.log(b); // ❌ ReferenceError — TDZ
let b = 10;
```

### Cách đặt tên biến trong JS

Các cách đặt tên:

- camelCase: `firstName`, `isLoggedIn`
- snake_case: `first_name`, `is_logged_in`
- kebab-case: `first-name`, `is-logged-in`
- PascalCase: `FirstName`, `IsLoggedIn`

> Thực tế:

- Ưu tiên camelCase cho biến và hàm.

## Kiểu dữ liệu trong Javascript

JS có 8 kiểu dữ liệu (7 kiểu nguyên thuỷ + 1 kiểu tham chiếu).

### 1. Kiểu nguyên thuỷ (Primitive)

| Kiểu        | Ví dụ                   | typeof           |
| ----------- | ----------------------- | ---------------- |
| `number`    | `42`, `3.14`, `NaN`     | `"number"`       |
| `string`    | `"Hello"`, `'Xin chào'` | `"string"`       |
| `boolean`   | `true`, `false`         | `"boolean"`      |
| `undefined` | `let x;`                | `"undefined"`    |
| `null`      | `let x = null;`         | `"object"` (lỗi) |
| `bigint`    | `9007199254740991n`     | `"bigint"`       |
| `symbol`    | `Symbol("id")`          | `"symbol"`       |

#### `number`

```js
let count = 100;
let price = 19.99;
let hex = 0xff; // 255
let infinity = 1 / 0; // Infinity
let notANumber = "abc" * 2; // NaN (Not a Number)
```

#### `string`

```js
let single = "Dùng nháy đơn";
let double = "Dùng nháy kép";
let template = `Template literal — có thể viết ${single}`;
// Kết quả: "Template literal — có thể viết Dùng nháy đơn"

let multiLine = `
  Dòng 1
  Dòng 2
`;
```

> Template literal (`` ` ` ``) cho phép nội suy (`${...}`) và xuống dòng — ưu tiên dùng.

#### `boolean`

```js
let isActive = true;
let isAdmin = false;
let isEven = 10 % 2 === 0; // true
```

#### `undefined` vs `null`

```js
let a; // undefined — chưa gán giá trị
let b = null; // null — cố tình "không có gì"
```

| Giá trị     | Ý nghĩa                           |
| ----------- | --------------------------------- |
| `undefined` | Biến được khai báo nhưng chưa gán |
| `null`      | "Không có giá trị" — chủ động gán |

> Thực tế:

- Ưu tiên null cho giá trị rỗng có chủ đích (rất phổ biến trong code hiện đại).
- Dùng undefined chủ yếu để kiểm tra giá trị mặc định hoặc thuộc tính không tồn tại.

### 2. Kiểu tham chiếu (Reference) — Object

```js
// Object literal
let person = {
  name: "An",
  age: 22,
  isStudent: true,
};

// Array (thực chất là object)
let colors = ["đỏ", "xanh", "vàng"];

// Function (cũng là object)
function greet() {
  console.log("Xin chào!");
}
```

### `typeof` — kiểm tra kiểu dữ liệu

```js
console.log(typeof 42); // "number"
console.log(typeof "JS"); // "string"
console.log(typeof true); // "boolean"
console.log(typeof undefined); // "undefined"
console.log(typeof null); // "object" (lỗi lịch sử — null là primitive)
console.log(typeof {}); // "object"
console.log(typeof []); // "object"
console.log(typeof function () {}); // "function"
```

### So sánh Primitive và Reference

```js
// Primitive — so sánh giá trị
let x = 10;
let y = 10;
console.log(x === y); // true

// Reference — so sánh tham chiếu (địa chỉ vùng nhớ)
let objA = { value: 10 };
let objB = { value: 10 };
let objC = objA;
console.log(objA === objB); // false — khác vùng nhớ
console.log(objA === objC); // true — cùng tham chiếu
```

| Đặc điểm | Primitive       | Reference (Object)        |
| -------- | --------------- | ------------------------- |
| Lưu tại  | Stack (giá trị) | Heap (tham chiếu đến)     |
| So sánh  | Theo giá trị    | Theo tham chiếu (địa chỉ) |
| Gán biến | Copy giá trị    | Copy tham chiếu           |

## Toán tử và biểu thức

### 1. Toán tử số học (Arithmetic)

| Toán tử | Ý nghĩa        | Ví dụ            |
| ------- | -------------- | ---------------- |
| `+`     | Cộng           | `5 + 3 -> 8`     |
| `-`     | Trừ            | `5 - 3 -> 2`     |
| `*`     | Nhân           | `5 * 3 -> 15`    |
| `/`     | Chia           | `5 / 2 -> 2.5`   |
| `%`     | Chia lấy dư    | `5 % 2 -> 1`     |
| ``      | Luỹ thừa (ES6) | `2  3 -> 8`      |
| `++`    | Tăng 1         | `let a = 1; a++` |
| `--`    | Giảm 1         | `let b = 5; b--` |

```js
let sum = 10 + 5; // 15
let remainder = 17 % 3; // 2
let power = 2  4; // 16

let counter = 0;
counter++; // counter = 1
counter--; // counter = 0
```

### 2. Toán tử gán (Assignment)

| Toán tử | Tương đương | Ví dụ              |
| ------- | ----------- | ------------------ |
| `=`     | `x = y`     | `let x = 5`        |
| `+=`    | `x = x + y` | `x += 3` -> x = 8  |
| `-=`    | `x = x - y` | `x -= 2` -> x = 6  |
| `*=`    | `x = x * y` | `x *= 2` -> x = 12 |
| `/=`    | `x = x / y` | `x /= 3` -> x = 4  |
| `%=`    | `x = x % y` | `x %= 3` -> x = 1  |
| `=`     | `x = x  y`  | `x = 2` -> x = 1   |

```js
let score = 10;
score += 5; // 15
score *= 2; // 30
```

### 3. Toán tử so sánh (Comparison)

Trả về `boolean` (`true` / `false`).

| Toán tử | Ý nghĩa                       | Ví dụ                |
| ------- | ----------------------------- | -------------------- |
| `==`    | Bằng nhau (không check kiểu)  | `5 == "5" -> true`   |
| `===`   | Bằng nhau tuyệt đối (cả kiểu) | `5 === "5" -> false` |
| `!=`    | Khác nhau (không check kiểu)  | `5 != "5" -> false`  |
| `!==`   | Khác nhau tuyệt đối           | `5 !== "5" -> true`  |
| `>`     | Lớn hơn                       | `10 > 5 -> true`     |
| `<`     | Nhỏ hơn                       | `10 < 5 -> false`    |
| `>=`    | Lớn hơn hoặc bằng             | `5 >= 5 -> true`     |
| `<=`    | Nhỏ hơn hoặc bằng             | `4 <= 5 -> true`     |

> Luôn dùng `===` và `!==` thay vì `==`/`!=` để tránh lỗi ép kiểu ngầm định.

```js
console.log(5 == "5"); // true  (ép kiểu)
console.log(5 === "5"); // false (khác kiểu)

console.log(null == undefined); // true  (đặc biệt)
console.log(null === undefined); // false
```

### 4. Toán tử logic (Logical)

| Toán tử | Ý nghĩa  | Ví dụ                              |
| ------- | -------- | ---------------------------------- |
| `&&`    | VÀ       | `true && false -> false`           |
| `\|\|`  | HOẶC     | `true \|\| false -> true`          |
| `!`     | PHỦ ĐỊNH | `!true -> false`                   |
| `??`    | Nullish  | `null ?? "mặc định" -> "mặc định"` |

```js
let isLoggedIn = true;
let isAdmin = false;

console.log(isLoggedIn && isAdmin); // false
console.log(isLoggedIn || isAdmin); // true
console.log(!isLoggedIn); // false
```

#### Short-circuit evaluation

```js
// && — nếu vế trái false thì không đánh giá vế phải
false && console.log("Không chạy"); // không in gì

// || — nếu vế trái truthy thì không đánh giá vế phải
true || console.log("Không chạy");

// Dùng || để gán giá trị mặc định (cách cũ)
let username = inputValue || "Khách";

// Dùng ?? — chỉ fallback khi null/undefined
let displayName = userInput ?? "Mặc định";
```

| `??` (Nullish coalescing) | Fallback chỉ khi `null` hoặc `undefined`     |
| ------------------------- | -------------------------------------------- |
| `0 ?? "x" -> 0`           | `0` là falsy nhưng không phải null/undefined |
| `"" ?? "x" -> ""`         | `""` tương tự                                |
| `null ?? "x" -> "x"`      | null -> fallback                             |

### 5. Toán tử chuỗi (String operator) `+`

Toán tử `+` với chuỗi thực hiện nối chuỗi:

```js
let firstName = "Nguyễn";
let lastName = "An";
let fullName = firstName + " " + lastName; // "Nguyễn An"

// Template literal tốt hơn
let greeting = `Xin chào ${fullName}!`; // "Xin chào Nguyễn An!"
```

### 6. Toán tử ba ngôi (Ternary)

Cú pháp rút gọn của `if/else`:

```js
let age = 18;
let status = age >= 18 ? "Trưởng thành" : "Chưa đủ tuổi";
console.log(status); // "Trưởng thành"

// Tương đương:
// let status;
// if (age >= 18) {
//   status = "Trưởng thành";
// } else {
//   status = "Chưa đủ tuổi";
// }
```

### Ép kiểu (Type coercion) — ngầm định

JS tự động ép kiểu trong một số tình huống:

```js
console.log("5" - 2); // 3  (string -> number)
console.log("5" + 2); // "52" (number -> string — + ưu tiên nối chuỗi)
console.log("5" * "2"); // 10 (cả hai sang number)
console.log(+"5"); // 5 (unary + ép sang number)
```

> `+` là ngoại lệ: nếu có string, nó ưu tiên nối chuỗi; các toán tử khác (`-`, `*`, `/`, `%`) ưu tiên ép sang number.

### Ép kiểu tường minh

```js
// String -> Number
console.log(Number("42")); // 42
console.log(parseInt("42px")); // 42
console.log(parseFloat("3.14")); // 3.14

// Number -> String
console.log(String(100)); // "100"
console.log((100).toString()); // "100"

// Anything -> Boolean
console.log(Boolean(1)); // true
console.log(Boolean(0)); // false
console.log(Boolean("")); // false
console.log(Boolean("abc")); // true
```

#### Truthy và Falsy

| Falsy (6 giá trị) | Truthy (mọi thứ còn lại)      |
| ----------------- | ----------------------------- |
| `false`           | `true`                        |
| `0` (và `-0`)     | Mọi số khác 0                 |
| `""` (chuỗi rỗng) | Chuỗi không rỗng              |
| `null`            | Mọi object (kể cả `[]`, `{}`) |
| `undefined`       | Function                      |
| `NaN`             |                               |

## Tóm tắt buổi học

1. JS là ngôn ngữ lập trình web — thông dịch, động, chạy trên browser/server.
2. 3 cách tích hợp: Inline (ít dùng), Internal (`<script>` trong HTML), External (file `.js` riêng — khuyến nghị).
3. Biến: `let` (block scope, gán lại được), `const` (block scope, không gán lại), tránh `var`. Khái niệm hoisting và TDZ.
4. Kiểu dữ liệu: 7 primitive (`number`, `string`, `boolean`, `undefined`, `null`, `bigint`, `symbol`) + Object. Phân biệt primitive (so sánh giá trị) và reference (so sánh tham chiếu).
5. Toán tử: số học (`+`, `-`, `*`, `/`, `%`, ``), gán (`=`, `+=`, …), so sánh (luôn dùng `===`/`!==`), logic (`&&`, `||`, `!`, `??`), ternary (`? :`), chuỗi (`+`). Ép kiểu ngầm định và tường minh, truthy/falsy.
