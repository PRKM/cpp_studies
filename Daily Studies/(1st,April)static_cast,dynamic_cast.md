# static_cast, dynamic_cast

I was curious about that type cast operator while making Game Engine, based on Direct2D, with my classmate. Frankly, I was using these operators not understanding about this.

Anyway, let's move on actual context.



## static_cast

`static_cast` operator is alike C language's cast operator. But, this is not stronger then that.

**Usage of `static_cast`**

* casting between real number(like `float`) and Integer, or between Integer and enum
* casting between classes that have inheritance relationship.
* casting void pointer type to other pointer type.

But this cast operator can't cast pointer to other pointer type.

`static_cast` doesn't check type on runtime. So in some case, this operator is **not safer** than `dynamic_cast`. But it means this is more **faster** than `dynamic_cast`.

_This is an example for `static_cast`  :_

```c++
class B {};

class D : public B {};

void foo(B* pb, D* pd){
  D* pd2 = static_cast<D*>(pb); // this is not safe. Class D can has
  							  // fields or methods that are not in B.
  
  B* pb2 = static_cast<B*>(pd); // this is safe. D always has B members.
}
```

`static_cast` doesn't check type on runtime, so casting from `pb` to `pd2` can be dangerous. for example, if you call a member, that are not in `B`, but in `D`, **access violation** can be occured.

Anyway, you should use `static_cast` when you think that code will work correctly.



## dynamic_cast

`dynamic_cast` has some difference with `static_cast`.

`static_cast` doesn't check type on runtime, but `dynamic_cast` does. If casting with `dynamic_cast` is valid, it returns 0 and fails runtime. So this is safer than `static_cast`.

But there's not only benefits in this operator. `dynamic_cast` can be used for only **pointer** or **reference type**, and **overhead can be happened**, because of checking type on runtime.

The typical example of this operator is checking type(actually class) of an instance.

This is Example for this : 

```c++
using namespace System;  
  
void PrintObjectType( Object^o ) {  
   if( dynamic_cast<String^>(o) )  
      Console::WriteLine("Object is a String");  
   else if( dynamic_cast<int^>(o) )  
      Console::WriteLine("Object is an int");
}
  
int main() {
   Object^o1 = "hello";
   Object^o2 = 10;
  
   PrintObjectType(o1);
   PrintObjectType(o2);
}
```

It can be more confused when casting multiple-inherited instance.



![여러 상속을 보여 주는 클래스 계층 구조](https://i-msdn.sec.s-msft.com/dynimg/IC88529.jpeg)

Image above, instance of D can be casted to B or C safely. But if cast D to A, some conflicts can be happened.

Then, what should we do? This example shows the solution : 

```c++
class A {virtual void f();};  
class B {virtual void f();};  
class D : public B {virtual void f();};  
  
void f() {  
   D* pd = new D;  
   B* pb = dynamic_cast<B*>(pd);   // first cast to B  
   A* pa2 = dynamic_cast<A*>(pb);   // ok: unambiguous  
}  
```

