# Spring Boot

아래의 영상을 참고하여 작성

[https://www.youtube.com/watch?v=HAlcc-HMz7k&list=PLyebPLlVYXCiYdYaWRKgCqvnCFrLEANXt](https://www.youtube.com/watch?v=HAlcc-HMz7k&list=PLyebPLlVYXCiYdYaWRKgCqvnCFrLEANXt)

# 프로젝트 코드

[GitHub - softwarerbfl/firstproject](https://github.com/softwarerbfl/firstproject)

![캡처 PNG (1)](https://user-images.githubusercontent.com/77094833/147883560-7c19bb83-e1a5-42c7-938f-775230ae4f53.png)

main 폴더는 java 폴더와 resources폴더로 나누어진다.

## 코드 경로

- src
    - main
        - java
            - com.example.firstproject
                - **controller**
                    - ArticleController
                    - FirstController
                - **dto**
                    - ArticleForm
                - **entity (실제 dv의 테이블과 매칭될 클래스)**
                    - Article
                - **repository**
                    - ArticleRepository
                - FirstprojectApplication
        - resources
            - static(content들을 두는 곳. 보통 css나 js파일)
                - hello.html(사용은 안되지만 실습에서 잠깐 사용했던)
            - templates(주로 html파일들-여기서는 mustache파일)
                - articles
                    - new.mustache
                - layouts
                    - goodbye.mustache
                    - greetings.mustache
            - application.properties
    - test

# MVC모델

![캡처 PNG (2)](https://user-images.githubusercontent.com/77094833/147883600-6891e775-ae7a-4094-9f93-56f5e64cd7d1.png)

## View

컨트롤러가 사용자에게 보내주는 것 html, jsp

**템플릿**

- FreeMarker
- Groovy
- Thymeleaf
- **Mustache** → Mustache 플러그인 설치

![캡처 PNG (3)](https://user-images.githubusercontent.com/77094833/147883616-b98b55d9-0a25-40f1-a2ff-06727df72ada.png)

<aside>
💡 콘트롤러 클래스 내부에 `newArticleForm`이라는 메소드가 존재하는데, 이 메소드의 경우 `@GetMapping`이라는 annotation을 사용하여 local:8080/articles/new라는 url주소로 접속했을 때, 프로젝트 파일 내부에서 src/main/resources/templates/**articles/new.mustache**을 보여준다.  여기서 return해주어야하는 주소 부분은 `articles/new`이다. 즉 templates의 하위 주소.

</aside>

## Controller

주로 사용자의 요청을 처리한 후 지정된 뷰에 모델 객체를 넘겨주는 역할

1. 사용자들이 웹브라우저에서 ‘URI *’로 요청을 보내면, 그 요청을 컨트롤러가 받게된다.
2. 요청에 대한 응답(View)를 반환한다.

<aside>
💡 URI(Uniform Resource Identifier) : 위치로 찾아가게 하는 것이 아니라, 아이디로 매핑시켜놓는다.

</aside>

→ 사용자가 uri로 요청을 했을 때, 서버로 요청이 들어오면, uri가 매핑된 컨트롤러의 메소드가 실행된다. 

# Dto → ArticleForm

계층간 데이터 교환을 위한 객체

DB에서 데이터를 얻어 Service나 Controller등으로 보낼 때 사용하는 객체

**View**를 위한 클래스

`toEntity()`메소드를 통해서 DTO에서 필요한 부분을 이용해 Entity로 만든다.

![폼에서 입력받아 db에 저장되는 과정](https://user-images.githubusercontent.com/77094833/147883624-631a5d73-201f-4ce2-bb88-764be5778cad.PNG)

![캡처](https://user-images.githubusercontent.com/77094833/147883638-1f60f25d-7f7a-4f6e-bbf2-02fd1c969131.PNG)

# Entity → Article객체 생성

실제 DB에 쓰일 필드와 여러 엔티티간 연관관계를 정의

- `@Entity` : 테이블과 링크될 클래스
- `@Id` : 해당 테이블의 PK필드
- `@GeneratedValue` : PK의 생성 규칙 표시
- `@Column` : 테이블의 칼럼임을 표시
- `@AllArgsConstructor` : 선언된 모든 필드를 파라미터로 갖는 생성자를 자동으로 생성해준다.
- `@NoArgsContructor` : 파라미터가 아예 없는 기본 생성자를 자동으로 생성해준다.
- `@ToString` : 해당 클래스에 선언된 필드들을 모두 출력할 수 있는 toString메소드를 자동으로 생성해준다.

![캡처 PNG (4)](https://user-images.githubusercontent.com/77094833/147883655-a5abcc2d-0acc-42d6-bf63-a70da1bb51a5.png)

# Repository → 인터페이스

 Entity에 의해 생성된 DB에 접근하는 메소드를 사용하기 위한 인터페이스이다. 

아래의 경우 `CrudRepository`를 상속받아 데이터 베이스에 어떤 값을 넣거나, 조회하거나 삭제(Create, Update, Read, Delete)할 수 있게 됩니다. 이 때 뒤에 <Article, Long>이 의미하는 것은 `**<대상으로 지정할 Entity, 해당 Entity의 PK타입>**`을 의미합니다.

![캡처 PNG (5)](https://user-images.githubusercontent.com/77094833/147883669-e3a219c2-48ea-4ab6-b32d-4d107e382b22.png)

- 실제로 DB에 접근하는 객체
- Service와 DB를 연결하는 고리
- SQL을 사용해서 DB에 접근한 후 CRUD API를 제공

# 프로그램 Refactoring by using annotation

## controller 파일 refactoring

![controller 리팩토링 전](https://user-images.githubusercontent.com/77094833/147883679-33d7535d-7ee4-4273-861d-c1e2cad0a224.PNG)

![controller 리팩토링 후](https://user-images.githubusercontent.com/77094833/147883684-ef182418-9c7c-48b5-828c-d3b286d45fb6.PNG)

## dto 파일 refactoring

![dto리팩토링 전](https://user-images.githubusercontent.com/77094833/147883690-acba6597-15b0-4d9a-9f01-fe6f79920f9b.PNG)

![dto리팩토링 후](https://user-images.githubusercontent.com/77094833/147883693-b7c82aa7-ea32-4f1d-bc35-76005d8dd43e.PNG)

## entity 파일 refactoring

![entity 리팩토링 전](https://user-images.githubusercontent.com/77094833/147883697-e6719abc-8a72-43e1-aa64-49bb49239fae.PNG)

![entity 리팩토링 후](https://user-images.githubusercontent.com/77094833/147883702-a63e0442-001a-4f94-9077-a86aab61162c.PNG)
