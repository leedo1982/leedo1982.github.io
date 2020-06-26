---
layout  : wiki
title   : RollbackExecption
summary : rollback
date    : 2019-02-14 12:35:53 +0900
updated : 2020-06-26 14:16:05 +0900
tag     : failover
toc     : true
public  : true
parent  : 
latex   : false
---
* TOC
{:toc}

# 증상
```
org.springframework.transaction.UnexpectedRollbackException: JTA transaction unexpectedly rolled back (maybe due to a timeout); nested exception is javax.transaction.RollbackException: One or more resources refused to commit (possibly because of a timeout in the resource - see the log for details). This transaction has been rolled back instead.
at org.springframework.transaction.jta.JtaTransactionManager.doCommit(JtaTransactionManager.java:1037)
at org.springframework.transaction.support.AbstractPlatformTransactionManager.processCommit(AbstractPlatformTransactionManager.java:746)
at org.springframework.transaction.support.AbstractPlatformTransactionManager.commit(AbstractPlatformTransactionManager.java:714)
at org.springframework.transaction.interceptor.TransactionAspectSupport.commitTransactionAfterReturning(TransactionAspectSupport.java:533)
at org.springframework.transaction.interceptor.TransactionAspectSupport.invokeWithinTransaction(TransactionAspectSupport.java:304)
at org.springframework.transaction.interceptor.TransactionInterceptor.invoke(TransactionInterceptor.java:98)
at org.springframework.aop.framework.ReflectiveMethodInvocation.proceed(ReflectiveMethodInvocation.java:186)
at org.springframework.aop.framework.CglibAopProxy$DynamicAdvisedInterceptor.intercept(CglibAopProxy.java:688)
at com.dealicious.ssmadmin.service.WarehousingOddServiceImpl$$EnhancerBySpringCGLIB$$a6ae6233.getRsAddPrice(<generated>)



Caused by: com.atomikos.icatch.RollbackException: One or more resources refused to commit (possibly because of a timeout in the resource - see the log for details). This transaction has been rolled back instead.
at com.atomikos.icatch.imp.ActiveStateHandler.prepare(ActiveStateHandler.java:202)
at com.atomikos.icatch.imp.CoordinatorImp.prepare(CoordinatorImp.java:523)
at com.atomikos.icatch.imp.CoordinatorImp.terminate(CoordinatorImp.java:687)
at com.atomikos.icatch.imp.CompositeTransactionImp.commit(CompositeTransactionImp.java:282)
at com.atomikos.icatch.jta.TransactionImp.commit(TransactionImp.java:172)
```

# 해결
slow 쿼리가 있는지 로그를 보며 확인한다.

# 배운점
JTA를 처음 적용하면서 학습의 부족으로, 에러가 발생하자 JTA의 문제라고 전제하고 해결방법을 찾았다.
무조건 로그를 믿었어야 했는데 방법이 부족했던거 같다.

" 집중하다 보면 주변을 못보고 파고들어 가게 된다.
그럴때 휴식을 가지고 주변을 바라보면 더 많은 것이 보일 것이다.  "

