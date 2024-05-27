---
title: Spring Boot - 게시판 CRUD
date: 2024-05-21 16:00:00 +09:00
categories: [Back-End, Spring Boot]
tags: [Back-End, Java, Spring Boot, CRUD]
---

## Ⅰ. 게시글 Create

### 1. 컨트롤러 생성

- `GET` 방식으로 `/articles/new` 주소 요청 시 `templates/articles/new.mustache` 문서 전송
- `POST` 방식으로 요청 시 DTO로 데이터를 받아온 후 엔티티로 변환, 최종적으로 DB에 저장
- DB접근하여 CRUD와 같은 로직을 수행하는 레포지토리 객체를 `@Autowired`를 통해 자동으로 의존성 주입

```java
@Controller
public class ArticleController {

  @Autowired
  private ArticleRepository articleRepository;
 
  // Get 매핑
  @GetMapping("/articles/new")
  public String newArticleForm() {
      return "articles/new";
  }

  // Post 매핑
  @PostMapping("/articles/create")
  public String createArticle(ArticleForm form) {

    // 1. DTO를 엔티티로 변환
    Article article = form.toEntity();
    System.out.println(article.toString());

    // 2. 래퍼지토리로 엔티티를 DB에 저장
    Article savedArticle = articleRepository.save(article);
    System.out.println(savedArticle);

    return "";
  }
}
```

---

### 2. DTO 생성

- 앞선 컨트롤러에서 `POST` 방식의 요청시 필요한 DTO(Data Transfer Object) 클래스 정의
- 서버에 요청하는 데이터와는 그 키의 이름과 값의 타입이 같아야 한다.
- DTO 객체를 엔티티로 변환하는 메서드 또한 추가

```java
@AllArgsConstructor
@ToString
public class ArticleForm {
  private String title;
  private String content;
  
  public Article toEntity() {
    return new Article(null, title, content);
  }
}
```

---

### 3. JPA와 엔티티 및 레포지토리

> JPA : JAVA Persistence API  
> 자바 언어로 DB에 명령을 내리는 도구  
> 데이터를 객체 지향적으로 관리할 수 있게 해줌

- 엔티티란 자바 객체를 DB가 이해할 수 있게 만든 것
- 클래스에 `@Entity` 어노테이션을 붙여 엔티티로 선언한다.
- `id` 필드는 `@Id`를 통해 주요키로 선언하고, `@GeneratedValue`를 통해 자동 생성 선언
- 나머지 필드는 `@Column`를 통해 엔티티의 속성으로 선언

```java
@AllArgsConstructor
@ToString
@Entity
public class Article {
  
  @Id
  @GeneratedValue
  private Long id;

  @Column
  private String title;

  @Column
  private String content;
}
```

- `CrudRepository` : Spring Data JPA가 제공하는 인터페이스로 기본적인 CRUD 작업을 위한 메서드를 포함
- `CrudRepository<Article, Long>`: `Article`은 관리할 엔티티 타입이고, `Long`은 엔티티의 식별자(ID)의 타입

```java
import org.springframework.data.repository.CrudRepository;

import kr.or.kosa.springproject1.entity.Article;

public interface ArticleRepository extends CrudRepository<Article, Long> {
  
}
```

---

### 4. 폼데이터를 통한 데이터 전송 후 조회

- 데이터를 Create를 하기 위한 폼데이터 입력 페이지
- `form` 태그의 `action` 속성으로 요청 url 설정
- `method` 속성을 통해 요청 방식 설정
- `input` 태그의 `name`은 관련 DTO의 필드명과 같게 해야함

```html
{% raw %}
{{>layouts/header}}
<form class="container" action="/articles/create" method="post">
    <div class="mb-3">
        <label class="form-label">제목</label>
        <input type="text" class="form-control" name="title">
    </div>
    <div class="mb-3">
        <label class="form-label">내용</label>
        <textarea class="form-control" rows="3" name="content"></textarea>
    </div>
    <button type="submit" class="btn btn-primary">Submit</button>
</form>
{{>layouts/footer}}
{% endraw %}
```

- 현재 이용하고 있는 h2 데이터베이스를 조회하면 데이터가 잘 저장되는 것을 확인할 수 있다.

<img src="/assets/img/post/backend/spring-boot/board_crud/create_01.png" width="100%" title="create_01" alt="create_01"/>  

<img src="/assets/img/post/backend/spring-boot/board_crud/create_02.png" width="100%" title="create_02" alt="create_02"/>  

---
<br>

## Ⅱ. 게시글 Read

### 1. 컨트롤러 - 단일 데이터 조회

url을 통해 게시글의 id값을 넘겨 개별 게시글에 접근

```java
// 중괄호 안에 i`를 넣어줌으로써 id는 변수로 사용
@GetMapping("/articles/{id}")
public String showArticle(@PathVariable Long id, Model model) {
  // Optional<Article> articleEntity = articleRepository.findById(id);
  Article articleEntity = articleRepository.findById(id).orElse(null);
  model.addAttribute("article", articleEntity);
  return "articles/detail";
}
```

`@PathVariable`
- url요청으로 들어온 전달값을 컨트롤러의 매개변수로 가져오는 어노테이션

`findById(id)`
- 해당 id 값을 가지고 있는 데이터를 반환

`orElse(null)`
- id 값으로 데이터를 찾을 때 해당 id 값이 없으면 null을 반환
- 타입 선언은 `Optional<Article>`로 대체하면 생략 가능

`model.addAttribute("article", articleEntity)`
- 가져온 엔티티 데이터를 모델에 등록
- 데이터를 뷰페이지에서 사용하기 위함

---

### 2. 컨트롤러 - 데이터 목록 조회

```java
@GetMapping("/articles")
public String showArticles(Model model) {
  List<Article> articleEntity = articleRepository.findAll();
  model.addAttribute("articleList", articleEntity);
  return "articles/index";
}
```

```java
public interface ArticleRepository extends CrudRepository<Article, Long> {
  @Override
  ArrayList<Article> findAll();
}
```

`findAll()`
- 해당 엔티티의 모든 데이터를 반환
- 반환 타입이 `Iterable`이기 때문에 위와 같이 오버라이드를 통해 반환 타입을 변경
- `(List<Article>)articleRepository.findAll()`와 같이 할당값을 다운캐스팅시키는 방법 존재
- `Iterable<Article> articleEntity` 와 같이 변수를 업캐스팅 시키는 것도 가능

---

### 3. 뷰페이지 - 단일 데이터 조회 페이지

```html
{% raw %}
<!-- detail.mustache -->
{{>layouts/header}}

<table class="table">
  <thead>
    <th scope="col">ID</th>
    <th scope="col">Title</th>
    <th scope="col">Content</th>
  </thead>
  <tbody>
    {{#article}}
    <tr>
      <td>{{id}}</td>
      <td>{{title}}</td>
      <td>{{content}}</td>
    </tr>
    {{/article}}
  </tbody>
</table>

{{>layouts/footer}}
{% endraw %}
```

<img src="/assets/img/post/backend/spring-boot/board_crud/read_01.png" width="100%" title="read_01" alt="read_01"/>

`#articleList`, `/articleList`
- 이중 중괄호 안 `#`과 `/`뒤에 모델로 넘긴 속성명을 입력하여 데이터에 접근
- 그 사이 이중 중괄호에는 엔티티의 속성명을 통해 속성값에 접근 가능

---

### 뷰페이지 - 목록 조회 페이지

```html
{% raw %}
<!-- index.mustache -->
{{>layouts/header}}

<table class="table">
  <thead>
    <th scope="col">ID</th>
    <th scope="col">Title</th>
    <th scope="col">Content</th>
  </thead>
  <tbody>
    {{#articleList}}
    <tr>
      <td>{{id}}</td>
      <td>{{title}}</td>
      <td>{{content}}</td>
    </tr>
    {{/articleList}}
  </tbody>
</table>

{{>layouts/footer}}
{% endraw %}
```

<img src="/assets/img/post/backend/spring-boot/board_crud/read_02.png" width="100%" title="read_02" alt="read_02"/>  

`#articleList`, `/articleList`
- 만약 배열 데이터라면 그 수만큼 반복됨

---
<br>

## Ⅲ. 게시글 Update

### 1. 컨트롤러 - 수정할 데이터 가져오기

- url을 통해 게시글의 id값을 넘겨받아 개별 게시글 정보 Read
- 가져온 정보를 model에 등록하여 페이지에 전달

```java
@GetMapping("/articles/{id}/edit")
public String showEdit(@RequestParam Long id, Model model) {
  // 수정할 데이터 가져오기
  Article articleEntity = articleRepository.findById(id).orElse(null);
  // 모델에 데이터 등록
  model.addAttribute("article", articleEntity);
  // 뷰페이지 설정
  return "articles/edit";
}
```

### 2. 컨트롤러 - 수정 데이터 받아오기 DB 갱신

- 폼 데이터로 보낸 데이터를 DTO로 수신
- 해당 id의 데이터가 DB에 존재하는지 확인
- 존재한다면 그대로 저장(기존 id의 데이터를 삭제하고, 새로 저장)
- 수정 완료 후 게시글 상세보기 페이지로 리다이렉트

```java
@PostMapping("/articles/edit")
public String editArticle(ArticleForm form) {
  // 수정된 데이터
  Article articleEntity = form.toEntity();
  Long id = articleEntity.getId();
  // 기존 데이터가 존재하는지 확인
  Article target = articleRepository.findById(id).orElse(null);
  // 기존 데이터 갱신
  if (target != null) {
    articleRepository.save(articleEntity);
  }
    
  return "redirect:/articles/" + id;
}
```

---

### 3. DTO - id 필드 추가

- 게시글 생성용 DTO에는 ID 필드가 없음
- 게시글 수정을 위해선 ID 정보가 필요하므로 id 필드 추가

```java
package kr.or.kosa.springproject1.dto;

import kr.or.kosa.springproject1.entity.Article;
import lombok.AllArgsConstructor;
import lombok.ToString;

@AllArgsConstructor
@ToString
public class ArticleEditFrom {
  private Long id;
  private String title;
  private String content;
  
  public Article toEntity() {
    return new Article(id, title, content);
  }
}
```

---

### 4. 뷰페이지 - 게시글 수정하기 페이지

```html
{% raw %}
<!-- edit.mustache -->

{{>layouts/header}}

<form class="container" action="/articles/edit" method="post">
    <input type="hidden" name="id" value="{{article.id}}">
    <div class="mb-3">
        <label class="form-label">제목</label>
        <input type="text" class="form-control" name="title" value="{{article.title}}">
    </div>
    <div class="mb-3">
        <label class="form-label">내용</label>
        <textarea class="form-control" rows="3" name="content" value="{{article.content}}"></textarea>
    </div>
    <button type="submit" class="btn btn-primary">Submit</button>
</form>

{{>layouts/footer}}
{% endraw %}
```

<img src="/assets/img/post/backend/spring-boot/board_crud/update_01.png" width="100%" title="update_01" alt="update_01"/>  
<img src="/assets/img/post/backend/spring-boot/board_crud/update_02.png" width="100%" title="update_02" alt="update_02"/>  

---
<br>

## Ⅳ. 게시글 Delete

### 1. 컨트롤러 - 대상 삭제

- url을 통해 게시글의 id값을 넘겨받아 삭제할 게시글에 접근
- 레포지토리의 `delete` 메서드를 이용하여 DB에서 삭제

```java
@GetMapping("/articles/{id}/delete")
public String deleteArticle(@PathVariable Long id) {
  // 삭제할 대상 가져오기
  Article target = articleRepository.findById(id).orElse(null);
  // 대상 엔티티 삭제
  if (target != null) articleRepository.delete(target);
  return "redirect:/articles";
}
```

---

### 2. 요청을 통한 게시글 삭제

<img src="/assets/img/post/backend/spring-boot/board_crud/delete_01.png" width="100%" title="delete_01" alt="delete_01"/> 

- url을 통해 삭제 컨트롤러 호출

<img src="/assets/img/post/backend/spring-boot/board_crud/delete_02.png" width="100%" title="delete_02" alt="delete_02"/>  

- 전체 게시글 조회 목록에서 해당 게시글이 삭제된 모습