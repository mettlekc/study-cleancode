# Chapter 06. 객체와 자료 구조
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
    public Point topLeft;
    public double side;    
}

public class Rectangle {
    public Point topLeft;
    public double height;
    public double width;
}

public class Circle {
    public Point center;
    public double radius;
}

public class Geometry {
    public final double PI = 3.14159265;
    
    public double area(Object shape) throws NoSuchShapeException {
        if (shape instanceof Square) {
            Square s = (Square) shape;
            return s.side * s.side;
        } else if (shape instanceof Rectangle) {
            Rectangle r = (Rectangle) shape;
            return r.height * r.width;
        } else if (shape instanceof Circle) {
            Circle c = (Circle) shape;
            return PI * c.radius * c.radius;
        }
    }
}
``` 
- 절차적인 도형
- 도형 추가 시 Geometry 모든 함수 수정 

```java
public class Square implements Shape {

    private Point topLeft;
    private double side;

    public double area() {
        return side * side;
    }
}

public class Rectangle implements Shape {

    private Point topLeft;
    private double height;
    private double width;

    public double area() {
        return height * width;
    }
}

public class Circle implements Shape {

    private Point center;
    private double radius;
    
    public final double PI = 3.14159265;

    public double area() {
        return PI * radius * radius;
    }
}
```
- 다형적인 도형
- 새 함수 추가할 때 전체 도형 수정

***모든 것이 객체라는 생각은 미신, 단순한 자료구조와 절차적인 코드가 가정 적합한 상황도 있다***

### 디미터 법칙
Heuristic, 모듈은 자신이 조작하는 객체의 속사정을 몰라야 한다는 법칙

##### 기차 충돌 (train wreck)
```java
final String outputDir = ctxt.getOptions().getScratchDir().getAbsoulutePath();
``` 
```java
Options opts = ctxt.getOptions();
File scratchDir = opts.getScratchDir();
final String outputDir = scratchDir.getAbsoulutePath();
```
- 디미터 법치 위반 여부
  - 객체라면 내부 구조를 숨겨야 하므로 디미터 법칙 위반
  - 자료구조라면 내부구조를 노출해야 하기 때문에 디미터 법칙 적용 안함

##### 잡종 구조
- 한 클래스 내에서 절반은 객체, 절반은 자료구조
- Feature Envy (기능 욕심)

##### 구조체 감추기
(가정) ctxt, options, scratchDir이 객체였다면
```java
ctxt.getAbsolutePathOfScratchDirectoryOption();
```
- ctxt 객체에 공개해야 하는 메서드가 많음

```java
ctxt.getScratchDirectoryOption().getAbsolutePath();
``` 
- (객체가 아닌) 자료구조를 반환한다고 가정

임시 디렉터리의 절대 경로가 필요한 이유?? (구조체)
```java
String outFile = outputDir + "/" + className.replace('.', '/') + ".class";
FileOutputStream fout = new FileOutputStream(outFile);
BufferedOutputStream bos = new BufferedOutputStream(fout);
```
- 임시 파일을 생성하기 위해
- ctxt 객체에서 임시파일 생성하도록 위임
```java
BufferedOutputStream bos = ctxt.createScratchFileStream(classFileName);
```

### 자료 전달 객체
DTO (Data Transfer Object)
- 공개 변수만 있고, 함수가 없는 클래스
- Bean 구조 (사이비 캡슐화)

##### 활성 레코드
- DTO의 특수한 형태
- save/find와 같은 탐색 함수 제공
- 결국, 자료구조. 비지니스 규칙을 담으면서 내부자료를 숨기는 객체를 따로 생성
