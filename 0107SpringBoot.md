# Spring Boot 11-13

# 데이터 조회하기 with JPA

![데이터 조회](https://user-images.githubusercontent.com/77094833/148404980-d7aa593c-75cb-40ac-98f8-926538028442.PNG)

1. `findById()` 를 사용하여 id로 데이터를 가져온다
2. 가져온 데이터를 모델에 등록 (`addAttribute()`사용)
3. 보여줄 페이지 설정(return할 페이지)

![id로 조회 PNG](https://user-images.githubusercontent.com/77094833/148405484-42d2e28f-3d54-4e6c-854b-c88cf39ac7f9.png)

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

![데이터 목록](https://user-images.githubusercontent.com/77094833/148405038-95e029d5-2f5f-4ff2-b37b-9a97ab629e47.PNG)

1. `findAll()`을 사용하여 모든 Article을 가져온다.
2. 가져온 Article묶음을 view로 전달(`addAttribute()`사용)
3. view 페이지 설정(return할 페이지)

![데이터 목록 조회](https://user-images.githubusercontent.com/77094833/148405086-79f7fc03-b3fa-4230-9f81-9bbff067e500.PNG)

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

![링크 예시](https://user-images.githubusercontent.com/77094833/148405135-9ea85664-50aa-4719-b57d-5615d92e74b1.PNG)


아래와 같이 index.mustache파일 즉 데이터 목록 페이지에서 새로운 Article을 생성할 수 있는 페이지로 이동할 수 있는 버튼을 만들 수 있으며 결과는 아래와 같다.

![결과](https://user-images.githubusercontent.com/77094833/148405157-787d686e-2340-495c-8458-fcbf1ffd435c.PNG)

## 리다이렉트

![리다이렉트](https://user-images.githubusercontent.com/77094833/148405183-676baf63-d814-4cf1-b23f-7202f891b83b.PNG)

![리다이렉트 코드](https://user-images.githubusercontent.com/77094833/148405205-d1903161-8dcb-4a42-ade0-b8a9c92100a7.PNG)

→ articles/create 페이지가 열렸을 때, articles/id번호 페이지로 이동시켜준다. 이 프로젝트에서는 submit버튼을 누르면 입력한 페이지의 id번호 페이지로 이동

<aside>
💡 `getId()`를 사용하기 위해서 Article 객체에 `@Getter` 어노테이션을 주어야한다.

</aside>
