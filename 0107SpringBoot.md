# Spring Boot 11-13

# 데이터 조회하기 with JPA

![데이터 조회.PNG](Spring%20Boot%2011-13%20d6cf5ecc63bf48b3b844aa5c0570d9bf/%EB%8D%B0%EC%9D%B4%ED%84%B0_%EC%A1%B0%ED%9A%8C.png)

1. `findById()` 를 사용하여 id로 데이터를 가져온다
2. 가져온 데이터를 모델에 등록 (`addAttribute()`사용)
3. 보여줄 페이지 설정(return할 페이지)

![id로 조회.PNG](Spring%20Boot%2011-13%20d6cf5ecc63bf48b3b844aa5c0570d9bf/id%EB%A1%9C_%EC%A1%B0%ED%9A%8C.png)

<aside>
💡 `orElse(null)`는 id를 못찾았을 때의 예외처리

</aside>

articles/show.mustache파일은 아래와 같다.

```html
<!--한 id의 Article목록을 보여주는 페이지-->
{{>layouts/header}}

<table class="table">
    <thead>
    <tr>
        <th scope="col">ID</th>
        <th scope="col">Title</th>
        <th scope="col">Content</th>
    </tr>
    </thead>

    <tbody>
    {{#article}}
    <tr>
        <th>{{id}}</th>
        <td>{{title}}</td>
        <td>{{content}}</td>
    </tr>
    {{/article}}
    </tbody>
</table>
<a href="/articles">Go To Article List</a>

{{>layouts/footer}}
```

# 데이터 목록조회

![데이터 목록.PNG](Spring%20Boot%2011-13%20d6cf5ecc63bf48b3b844aa5c0570d9bf/%EB%8D%B0%EC%9D%B4%ED%84%B0_%EB%AA%A9%EB%A1%9D.png)

1. `findAll()`을 사용하여 모든 Article을 가져온다.
2. 가져온 Article묶음을 view로 전달(`addAttribute()`사용)
3. view 페이지 설정(return할 페이지)

![데이터 목록 조회.PNG](Spring%20Boot%2011-13%20d6cf5ecc63bf48b3b844aa5c0570d9bf/%EB%8D%B0%EC%9D%B4%ED%84%B0_%EB%AA%A9%EB%A1%9D_%EC%A1%B0%ED%9A%8C.png)

articles/index.mustache파일은 아래와 같다.

```html
<!--전체 Article 목록을 보여주는 테이블-->
{{>layouts/header}}

<table class="table">
    <thead>
    <tr>
        <th scope="col">ID</th>
        <th scope="col">Title</th>
        <th scope="col">Content</th>
    </tr>
    </thead>

    <tbody>
    {{#articleList}}
        <tr>
            <th>{{id}}</th>
            <!--title에 해당 id에 해당하는 링크 걸어줌-->
            <td><a href="/articles/{{id}}">{{title}}</a></td>
            <td>{{content}}</td>
        </tr>
    {{/articleList}}
    </tbody>
</table>

<!--새로운 Article을 작성할 수 있는 페이지로 이동할 수 있는 버튼을 생성-->
<a href="/articles/new">New Article</a>

{{>layouts/footer}}
```

<aside>
💡 {{#articleList}}
        <tr>
            <th>{{id}}</th>
            <!--title에 해당 id에 해당하는 링크 걸어줌-->
            <td><a href="/articles/{{id}}">{{title}}</a></td>
            <td>{{content}}</td>
        </tr>
    {{/articleList}}

→ 이 코드는 **articleList**에 존재하는 코드들을 반복한다는 뜻으로, 여기서는  id, title, content가 article에 따라 계속 변하면서 출력 

</aside>

# 링크와 리다이렉트

## 링크

![링크 예시.PNG](Spring%20Boot%2011-13%20d6cf5ecc63bf48b3b844aa5c0570d9bf/%EB%A7%81%ED%81%AC_%EC%98%88%EC%8B%9C.png)

아래와 같이 index.mustache파일 즉 데이터 목록 페이지에서 새로운 Article을 생성할 수 있는 페이지로 이동할 수 있는 버튼을 만들 수 있으며 결과는 아래와 같다.

![결과.PNG](Spring%20Boot%2011-13%20d6cf5ecc63bf48b3b844aa5c0570d9bf/%EA%B2%B0%EA%B3%BC.png)

## 리다이렉트

![리다이렉트.PNG](Spring%20Boot%2011-13%20d6cf5ecc63bf48b3b844aa5c0570d9bf/%EB%A6%AC%EB%8B%A4%EC%9D%B4%EB%A0%89%ED%8A%B8.png)

![리다이렉트 코드.PNG](Spring%20Boot%2011-13%20d6cf5ecc63bf48b3b844aa5c0570d9bf/%EB%A6%AC%EB%8B%A4%EC%9D%B4%EB%A0%89%ED%8A%B8_%EC%BD%94%EB%93%9C.png)

→ articles/create 페이지가 열렸을 때, articles/id번호 페이지로 이동시켜준다. 이 프로젝트에서는 submit버튼을 누르면 입력한 페이지의 id번호 페이지로 이동

<aside>
💡 `getId()`를 사용하기 위해서 Article 객체에 `@Getter` 어노테이션을 주어야한다.

</aside>
