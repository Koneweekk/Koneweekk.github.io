---
title: Spring - Transaction
date: 2024-05-07 10:50:00 +09:00
categories: [Back-End, Spring]
tags: [Back-End, Java, Spring, Transaction]
---

### 1. 트랜젝션이란?

> 여러 개의 데이터베이스 작업을 하나의 논리적인 작업 단위로 묶는 것

- 모든 작업이 성공하거나 모든 작업이 실패할 경우 전체 작업을 롤백
- 데이터베이스의 일관성과 무결성을 유지하기 위해 사용

---

### 2. 트랜젝션 설정

servlet-context.xml 파일에 transaction bean을 추가한다. 

```xml
<bean id="transactionManager"  class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
<property name="dataSource" ref="driverManagerDataSource" /> 
</bean>

<!-- @Transactional 사용하기 위해 선행되어야할 bean -->
<tx:annotation-driven transaction-manager="transactionManager"/> 
```

---

### 3. 트랜젝션 선언

`@Transactional` 어노테이션을 통해 트랜젝션 선언하는 것이 가장 간편한 방법 중 하나
- 어노테이션이 적용되면 메서드 내에서 발생하는 모든 데이터베이스 작업은 하나의 트랜잭션으로 관리됨
- 예외가 발생하면 트랜잭션이 롤백

```java
@Transactional
public String noticeReg(Notice n, HttpServletRequest request) throws Exception {

  ...

  try {  // insert와 update를 하나의 트랜젝션 단위로 묶음
    NoticeDao noticedao = sqlsession.getMapper(NoticeDao.class);
    noticedao.insert(n);
    noticedao.updateOfMemberPoint("example");
    System.out.println("정상 : notice insert.... member update....");
  } catch (Exception e) {  // 예외가 발생하면 롤백
    System.out.println("transaction 발생");
    throw e;
  }

  return "redirect:notice.htm";
}
```