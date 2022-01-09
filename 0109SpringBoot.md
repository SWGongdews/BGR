# Spring Boot 14-16

## 새로 추가된 sql파일

```sql
INSERT INTO ARTICLE(id, title, content) values(1,'가가가가','1111');
INSERT INTO ARTICLE(id, title, content) values(2,'나나나나','2222');
INSERT INTO ARTICLE(id, title, content) values(3,'다다다다','3333');
```

# 수정 폼 만들기

![SmartSelect_20220110-031051_Drive](https://user-images.githubusercontent.com/77094833/148696562-063fa8a9-1854-4b02-b590-5586d7395b17.jpg)


```java
@GetMapping("/articles/{**id**}/edit")
    public String edit(**@PathVariable long id**, Model model){ //url경로에서 id를 가져온다
        //수정할 데이터를 가져오기!
        Article articleEntity = articleRepository.findById(**id**).orElse(null);
        //모델에 데이터를 등록!
        model.addAttribute("article", articleEntity);
        //뷰 페이지 설정
        return "articles/edit"; 
    }
```

<aside>
💡 `@GetMapping("/articles/{id}/edit")` 에서 {id}부분을 사용하기 위해서 함수의 파라미터로 `@PathVariable long id`를 추가

</aside>

1. 수정할 데이터를 가져온다
2. 모델에 데이터를 등록한다
3. 뷰 페이지를 설정한다

![edit 페이지 PNG](https://user-images.githubusercontent.com/77094833/148696402-758811c5-af42-4112-919e-40847de86cf5.png)

edit버튼을 눌렀을 때 페이지

```html
{{>layouts/header}}

<!--기존의 article정보를 변경하고싶을 때 이동하는 페이지-->
{{#article}}
<form class="container" action="/articles/update" method="post">
    <input name="id" type="hidden" value="{{id}}"/>
    <div class="mb-3">
        <label class="form-label">제목</label>
        **<input type="text" class="form-control" name="title" value="{{title}}">**
    </div>
    <div class="mb-3">
        <label class="form-label">내용</label>
        **<textarea class="form-control" rows="3" name="content">{{content}}</textarea>**
    </div>
    <button type="submit" class="btn btn-primary">Submit</button>
    <a href="/articles/{{id}}" class="btn btn-primary">Back</a>
</form>
{{/article}}

{{>layouts/footer}}
```

<aside>
💡 이 코드는 articles/edit.mustache 파일로 article 데이터 값을 변경할 때 나오는 페이지의 모양을 잡아준다.

중요한 것은 **제목은 value파라미터에 title**을 넣어주고, **내용은 textarea태그 사이에 content**를 넣어준다는 것. (즉 데이터를 DB에서 가져옴)

</aside>

# 데이터 수정하기

![SmartSelect_20220110-032419_Drive](https://user-images.githubusercontent.com/77094833/148696571-e4a69e67-4782-4428-b536-9e5f0739c413.jpg)


```java
@PostMapping("/articles/update")
    public String update(ArticleForm form){
        log.info(form.toString());
        //1 : DTO를 엔티티로 변환
        Article articleEntity=form.toEntity();
        log.info(form.toString());

        //2 : 엔티티를 DTO로 저장
        //2-1 : DB에서 기존 데이터를 가져온다
        Article target = articleRepository.findById(articleEntity.getId()).orElse(null);

        //2-2 : 기존 데이터에 값을 갱신한다
        if(target!=null){
            **articleRepository.save(articleEntity);**
        }

        //3 : 수정 결과 페이지로 리다이렉트
        return "redirect:/articles/"+articleEntity.getId();
    }
```

<aside>
💡 intellij가 `@PatchMapping`어노테이션을 지원하지 않아서 `@PostMapping`어노테이션으로 대체

articleRepository내부에 `save()`라는 메소드를 사용하여 DB에 엔티티 갱신(UPDATE)

</aside>

1. DTO를 엔티티로 변환
2. 엔티티를 DTO로 저장
    1. DB에서 기존 데이터를 가져옴
        
        ![수정 전](https://user-images.githubusercontent.com/77094833/148696422-d47a1225-8c46-48ff-94d0-52a30c53352f.PNG)

        수정하기 전
        
    2. 기존 데이터에 값을 갱신
        
        ![수정 후](https://user-images.githubusercontent.com/77094833/148696439-91643981-740e-4780-9168-95bed5c7a58b.PNG)

        제목 변경
        
3. 수정 결과 페이지(**articles/id)**로 REDIRECT
    
    ![submit까지](https://user-images.githubusercontent.com/77094833/148696467-b9607fe4-0850-4e04-ac4f-2f71898a7ecd.PNG)

    Submit버튼 클릭하여 redirect까지 성공
    

# 데이터 삭제하기

![SmartSelect_20220110-033914_Drive](https://user-images.githubusercontent.com/77094833/148696592-bb93fc02-b59a-4f9b-ae7b-df2069afca9b.jpg)

```java
@GetMapping("/articles/{id}/delete")
    public String delete(@PathVariable long id, RedirectAttributes rttr){ 
		//RedirectAttributes는 REDIRECT가 이루어질 때 사용
        log.info("삭제 요청이 들어왔습니다!!");

        // 1 : 삭제 대상을 가져온다
        Article target =articleRepository.findById(id).orElse(null);
        log.info(target.toString());

        // 2 : 대상을 삭제한다!
        if(target!=null){
            articleRepository.delete(target);
            rttr.addFlashAttribute("msg","삭제가 완료되었습니다");
            //redirect된 페이지에서 사용되는 휘발성 데이터
        }

        // 3 : 결과 페이지로 리다이렉트한다!
        return "redirect:/articles";
    }
```

<aside>
💡 `delete()`함수에서 articles로 REDIRECT하여 반환하게 되는데 저 주소에서는 articles/index.mustache를 return한다. 따라서 위에서 사용된 `addFlashAttribute()`라는 함수에 사용된 "삭제가 완료되었습니다"라는 문자열은 "msg"로 index.mustache에 전달된다.

</aside>

1. 삭제 대상을 가져온다
    
    ![삭제전](https://user-images.githubusercontent.com/77094833/148696478-70c50645-5858-4df4-94c3-1d2bbd3e424b.PNG)

2. 대상을 삭제한다
3. 결과 페이지로 REDIRECT한다
    
    ![삭제후](https://user-images.githubusercontent.com/77094833/148696482-51d93e10-d321-4dc7-9a9c-d63bc0c18da1.PNG)


→ msg는 alert태그

 해당 css는 코드 참고
