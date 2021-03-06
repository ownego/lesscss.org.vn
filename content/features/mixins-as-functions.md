> Trả về biến hoặc mixin từ mixin

Biến và mixin được khai báo trong một mixin sẽ được nhìn thấy và được sử dụng trong phạm vi gọi. Chỉ có một ngoại lệ, biến không được copy nếu selector gọi chứa một biến cùng tên. Chỉ những biến được khai báo trong phạm vị selector gọi mới được an toàn, các biến kế thừa từ phạm vi cha sẽ bị ghi đè.

Ví dụ:

```less
.mixin() {
  @width:  100%;
  @height: 200px;
}

.caller {
  .mixin();
  width:  @width;
  height: @height;
}

```
Kết quả:

```css
.caller {
  width:  100%;
  height: 200px;
}
```

Như vậy, biến khai báo trong một mixin sẽ giống như các giá trị trả về của nó, cho phép ta tạo ra những mixin hoạt động gần như một hàm.

Ví dụ:

```less
.average(@x, @y) {
  @average: ((@x + @y) / 2);
}

div {
  .average(16px, 50px); // "gọi" mixin
  padding: @average;    // sử dụng giá trị "trả về"
}
```

Kết quả:

```css
div {
  padding: 33px;
}
```

Các biến khai báo trực tiếp trong phạm vi gọi không thể bị đè. Tuy nhiên, biến trong phạm vi cha của phạm vi khai báo sẽ có thể bị đè:

````less
.mixin() {
  @size: in-mixin; 
  @definedOnlyInMixin: in-mixin;
}

.class {
  margin: @size @definedOnlyInMixin;
  .mixin(); 
}

@size: globaly-defined-value; // biến ở phạm vi cha - có thể bị đề
````

Kết quả:
````css
.class {
  margin: in-mixin in-mixin;
}
````

Cuối cùng, mixin khai báo trong mixin cũng hoạt động như biến trả về:
````less
.unlock(@value) { // mixin bọc ngoài
  .doSomething() { // mixin lồng trong
    declaration: @value;
  }
}

#namespace {
  .unlock(5); // mở khóa doSomething mixin
  .doSomething(); // các mixin lồng trong được copy vào đây và có thể sử dụng
}
````

Kết quả:
````css
#namespace {
  declaration: 5;
}
````
