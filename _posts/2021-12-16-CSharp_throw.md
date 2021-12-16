---
title: "C# 예외처리와 throw"
excerpt: "예외처리와 throw문에 대해 알아보자"

categories:
  - Algorithm
tags:
  - [CSharp, Theory, Exception, Advanced]

toc: true
toc_sticky: true

date: 2021-12-16
last_modified_at: 2021-12-16
---


## Exception 예외처리

`try`, `catch`, `finally` 키워드를 사용하여 Exception을 핸들링하고, `throw` 키워드를 통해 Exception을 만들어 던지거나 기존 예외를 다시 던질 수 있다(전달).

C#에서는 .NET Framework의 Exception 메커니즘에 따라 예외를 처리한다. System.Exception 클래스는 모든 예외의 Base 클래스이며 예외 처리는 이 Exception 객체를 기본으로 처리하게 된다.

만약 Exception이 발생했는데 이를 프로그램 내에서 처리하지 않으면(Unhandled Exception) 프로그램은 Crash하여 종료된다.

```csharp
try
{
    // 실행하고자 하는 작업
    Do Something();
}
catch
{
    // 에러 처리/로깅 등의 작업
    Log(ex);
    throw;
}
```


## try, catch, fianlly


try 블럭은 실제 실행하고 싶은 프로그램 명령문들을 갖는 블럭이다. 만약 여기서 어떤 에러가 발생하면, 이는 catch 문에서 잡히게 된다. catch문은 모든 Exception을 일괄적으로 잡거나 혹은 특정 Exception을 선별하여 잡을 수 있다.   

- 모든 Exception을 잡고 싶을 때
  - catch { ... } 
  - catch (Exception ex) { ... }
    - 모든 Exception의 베이스 클래스인 System.Exception를 잡으면 된다.
- 특정 Exception을 잡고 싶을 때
  - 해당 Exception Type을 catch 하면 된다.
    - catch (ArgumentException ex) { ... }  // Argument와 관련된 Exception을 잡음

finally는 예외 발생 여부와 상관없이 마지막에 반드시 실행되는 블럭이다. 예를 들어 try 블럭에서 SQL Connection 객체를 만든 경우, finally 블럭에서 Connection 객체의 Close() 메서드를 호출하면 항상 Connection 객체가 닫히게 된다.

```csharp

try
{
   //실행 문장들
   // DB 연결
   string strConn = "Data Source=(local);Initial Catalog=pubs;Integrated Security=SSPI;";
   SqlConnection conn = new SqlConnection(strConn);
   conn.Open();
   DoSomething();
}
catch (ArgumentException ex)
{
   // Argument 예외처리
}
catch (AccessViolationException ex)
{
   // AccessViolation 예외처리
}
finally
{
   // 마지막으로 실행하는 문장들
   conn.Close() // DB Connection 해제
}
```


## throw
catch문에서 잡은 Exception을 상위 호출자로 전달하고 싶은 경우에 throw를 사용한다.

- throw문을 사용하는 3가지 Case
  - throw문 다음에 catch에서 전달받은 Exception 객체를 쓰는 경우  ( throw ex; )
  - throw문 다음에 새로운 Exception 객체를 생성해서 전달하는 경우  ( throw new MyException(); )
  - throw문 다음에 아무것도 없는 경우 ( throw; )
   
      
### (1) throw문 다음에 catch에서 전달받은 Exception 객체를 쓰는 경우

```csharp
try {..}
catch(Exception ex) { throw ex; }
```

`catch (Exception ex)` 에서 전달받은 아규먼트 ex 를 사용하는 경우이다. 이러한 throw 방식은 ex 에 담긴 예외 정보를 보전하지만, Stack Trace 정보를 다시 리셋하기 때문에 throw문 이전의 콜스택(Call Stack) 정보를 유실하게 된다. 

### (2) throw문 다음에 새로운 Exception 객체를 생성해서 전달하는 경우

```csharp
try {..}
catch(MyException ex) { throw new MyException(ex.Message, ex); }
```

catch 에서 잡은 Exception을 Wrapping 하여 새로운 Exception을 전달할 때 사용하는데, 잘못 사용하면(InnerException을 포함하지 않으면) 기존 Exception 정보를 잃을 수 있다.
따라서, 이러한 방식을 사용하여 새로운 Exception 객체를 만들 때는 catch 에서 얻은 Exception 객체를 새 객체의 InnerException에 포함시켜 에러 정보를 보전하는 것이 좋다(위 예제와 같이).
   
InnerException의 StackTrace 속성은 어느 라인에서 에러가 발생했는지를 알려주는데, 이는 에러가 다른 메서드에서 발생했을 때는 물론 동일 메서드에서 발생했다 하더라도 정확히 어떤 라인에서 에러가 발생했는지를 알게 해 준다.

### (3) throw문 다음에 아무것도 없는 경우

```csharp
try {..}
catch(Exception ex) { throw; }
```

catch문에서 잡힌 Exception을 그대로 상위 호출 함수에게 전달하는 일(rethrow)을 한다. 즉, 에러를 발생시킨 콜스택 정보를 그대로 상위 호출 함수에 전달하려면 이렇게 throw; 를 호출하면 된다. 
   
한가지 주목할 점은, throw; 는 에러가 다른 메서드에서 발생했을 때는 그 에러가 발생한 다른 메서드의 위치를 포함하지만, 만약 throw문과 동일한 메서드에서 에러가 발생했다면 동일 메서드의 어느 라인에서 에러가 발생했는지는 포함하지 않는다.


```csharp
try
{
    // 실행 문장들
    DoSomething();
}
catch(IndexOutOfRangeException ex)
{
    // 새로운 Exception 생성하여 throw
    // ex 객체를 InnerException에 포함시킴
    throw new MyException("Invalid index", ex);
}
catch(FileNotFoundException ex)
{    
    bool success = Log(ex);
    if (!success)
    {
        // 기존 Exception을 throw
        // throw ex 까지의 이전 콜스택을 제거
        throw ex;
    }
}
catch(Exception ex)
{    
    Log(ex);
    // 발생된 Exception을 그대로 호출자에 전달
    throw;
}
```