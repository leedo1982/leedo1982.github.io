---
layout  : wiki
title   : JAVA 8 을 어떻게하면 더 효율적으로 사용할 것인가...
summary : 
date    : 2019-02-22 10:17:38 +0900
updated : 2019-02-22 10:22:04 +0900
tags    : java
toc     : true
public  : true
parent  : 
latex   : false
---
* TOC
{:toc}

# functional interface 를 이용해서 lazy evaluation 을 활용하자 
## Supplier
```
@FunctionalInterface
public interface Supplier<T> {

    T get();
}
```


## Consumer
@FunctionalInterface
public interface Consumer<T> {

    void accept(T t);

    default Consumer<T> andThen(Consumer<? super T> after) {
        Objects.requireNonNull(after);
        return (T t) -> { accept(t); after.accept(t); };
    }
}

## Predicate
```
@FunctionalInterface
public interface Predicate<T> {

    boolean test(T t);

    default Predicate<T> and(Predicate<? super T> other) {
        Objects.requireNonNull(other);
        return (t) -> test(t) && other.test(t);
    }

    default Predicate<T> negate() {
        return (t) -> !test(t);
    }
    
    default Predicate<T> or(Predicate<? super T> other) {
        Objects.requireNonNull(other);
        return (t) -> test(t) || other.test(t);
    }
    
    static <T> Predicate<T> isEqual(Object targetRef) {
        return (null == targetRef)
                ? Objects::isNull
                : object -> targetRef.equals(object);
    }
}
```

메소에서 판단을 기준으로 메서들의 실행이 필요할 때 function 함수로 param을 사용하면
lazy evaluation 이 발생하여 좀 더 효율적인다.

