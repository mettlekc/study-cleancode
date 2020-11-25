# 객체와 자료 구조
- private 변수를 get/set 함수로 public 공개하는 이유?

### 자료 추상화
```java
public class Point {
    public double x;
    public double y;
}
```
- 직교좌표계
- 구현을 노출
- 객체

```java
public interface Point {
    double getX();
    double getY();
    void setCartesian(double x, double, y);
    double getR();
    double getTheta();
    void setPolar(double r, double theta);
}
```
- 직교좌표계? 극좌표계?
- 자료구조를 명백하게 표현
- set은 두 값을 한번에 설정, get은 개별
- 자료구조

***구현을 감추기 위해서는 추상화가 필요***

```java
public interface Vehicle {
    double getFuelTankCapacityInGallons();
    double getGallonsOfGasoline();
}
```
- 정보의 세세한 공개
```java
public interface Vehicle {
    double getPercentFuelRemaining();
}
```
- 추상적 개념 전달

### 자료/객체 비대칭

객체/자료구조
- 객체 : 추상화 뒤로 자료를 숨긴 채 자료를 다루는 함수만 공개
- 자료구조 : 자료를 그대로 공개하며 별다른 함수 제공 안함

```java
public class Square {
    
}

public class Rectangle {
    
}

public class Circle {
    
}

public class Geometry {
    
}
``` 
- 절차적인 도형
- 도형 추가 시 Geometry 모든 함수 수정 

```java
public class Square implements Shape {



    public double area() {
        
    }
}

public class Rectangle implements Shape {



    public double area() {
        
    }
}

public class Circle implements Shape {



    public double area() {
        
    }
}
```
- 다형적인 도형
- 새 함수 추가할 때 전체 도형 수정

***모든 것이 객체라는 생각은 미신, 단순한 자료구조와 절차적인 코드가 가정 적합한 상황도 있다***
