---
layout  : wiki
title   : 연결리스트 해법
summary : 
date    : 2018-07-21 13:37:40 +0900
updated : 2018-08-08 14:21:41 +0900
tags    : 
toc     : true
public  : false
parent  : 
latex   : false
---
* TOC
{:toc}

# 연결리스트 해법
### 2.1 중복없애기: 정렬되어 있지 않은 연결리스트가 주어졌을때 이 리스트에서 중복되는 원소를 제거하는 코드를 작성

간단히 해시테이블을 사용하여 처리
```
void deleteDups(LinkedListNode n){
    HashSet set = new HashSet();
    LinkedListNode previous = null;
    
    while(n != null){
        if(set.contains(n.data)) {
            previous.next = n.next;
        }else{
            set.add(n.data);
            previous = n;
        }
        n = n.next;
    }
}
```
시간복잡도 O(N) , N은 연결리스트의 길이


 연관문제: 임시버퍼를 사용할수 없다면 어떻게 풀까?
 두개의 포인터를 사용해서 문제를 해결할수 있다. current 포인터는 연결리스트를 순회하고, runner 포인터는 그 뒤어 중복되는 원소가 있는지 확인
```
void deleteDups(LinkedListNode head){
    LinkedListNode current = head;
    while(current != null){
    
        // 값이 같은 다른 노드를 모두 제겋ㄴ다.
        LinkedListNode runner = current;
        while(runner.next != null){
            if(runner.next.data == current.data){
                runner.next = runner.next.next;
            }else{
                runner = runner.next;
            
            }
        }
        current = current.next;
    }
}
```
공간복잡도 O(1)  시간복잡도 O(N^2)

### 2.2 뒤에서 k 번째 원소구하기: 단방향 연결리스트가 주어졌을 때 뒤어서 k번째 원소를 찾는 알고리즘을 구현하라.
 ==> 재귀와 비재귀 모두 알아볼것이다. 재귀는 깔끔하지만 최적이 아닐때가 많다.

여기서는 k = 1 일 때 마지막 원소를 반환하고 k = 2 일때 뒤에서 두번째 원소를 반환하는 식으로 k 를 정의할 것이다.
물론 k = 0 일때 마지막 원소를 반환하는 식으로 정의하도 똑같이 동작한다.

solution 1. 연결리스트 길이를 아는 경우
맨마지막 원소에서 k 번째 원소는 (length-k) 번째 원소가 된다.
따라서 단순히 연결리스트를 순회해서 이원소를 찾으면 되나, 원하는 결과는 아닐 것이다.

solution 2. 재귀적 방법
연결 리스트를 재귀적으로 순회한다.
마지막원소를 만나면 0으로 설정된 카운터를 반환한다. 
재귀적으로 호출된 부모 메서드는 그 값에 1을 더한다. 그러다 카운터 값이 k 가 되는 순간, 뒤어세 k번째 원소에 도달한다.
스택을 통해 정수값을 전달하는 방법이 있다면, 이해법은 정말로 짧고 아름답게 구현할 수 있다.
하지만 불행히도 노드와 카운터 값을 동시에 반환할 수가 없다. 

방법 A. 문제를 수정해서 맨 뒤에서 k번째 원소의 값을 출력하도록 바꾸면 된다. 그러면 카운터 값만 반환해도 된다.

```
int printKthToLast(LinkedListNode head, int k){
    if(head == null){ return 0; }
    int index = printKthToLast(head.next, k) + 1;
    if(index == k){
        System.out.println(k + "th to last node is" + head.data);
    }

    return index ;
}
```
물론 이 방법은 면접관이 허락한 경우에만 가능하다.


방법 B : C++ 로구현(pass)
방법 C : Wrapper 클래스 구현
    앞에서 카운터와 첨자를 동시에 반환할 수 없는 것이 문제 라고 했다. 만약 카운터에 값을 간단한 클래스로 감쌀 수 있다면(아니면 원소가 딱하나인 배열을 쓰거나), 참조에 의한 값 전달을 흉내낼 수 있다.

```
class Index{
    pulbic int value = 0;
}

LinkedListNode kthToLast(LinkedListNode head, int k){
    Index idx = new Index();
    return kthToLast(head, k, idx);
}

LinkedListNode kthToLast(LinkedListNode head, int k, Index idx){
    if(head == null){ return null; }
    LinkedListNode node = kthToLast(head.next, k, idx);
    idx.value = idx.value + 1;
    if(idx.value == k){
        return head;
    }
    return node;
}
```
재귀적인 방법은 재귀호출을 사용하므로 O(n)의 공간을 사용한다.

solution 3. 순환적 방법
직관적이지는 않지만 좀 더 최적인 방법은, 순환적으로 푸는 것이다. 
```
LinkedListNode nthToLast(LinkedListNode head, int k){
    LinkedListNode p1 = head;
    LinkedListNode p2 = head;
    
    // p1을 k노드만큼 이동시킨다.
    for(int i= 0; i < k ; i++){
        if(p1 ==null) return null; // out of bounds 
        p1 = p1.next;
    }

    // p1과 p2를 함께 움직인다. p1이 끝에 도달하면, p2는 LENGTH-k 번째 원소를 가르키게 된다.
    while(p1 != null){
        p1 = p1.next;
        p2 = p2.next;
    }
    return p2;
}
```
시간은 O(n) 공간은 O(1) 소요

### 2.3 중간 노드 삭제 : 단방향 연결리스트가 주어졌을 때 중간(정확히 가운데 노드일 필요는 없고, 처음과 끝 노드만 아니면 된다.)에 있는 노드 하나를 삭제하는 알고리즘구현하라.단, 삭제할 노드에만 접근할 수 있다.

입력: 연결리스트 a->b->c->d->e 에서 노드 c
결과: 아무것도 반환할 필요는 없지만, 결과로 연결리스트 a->b->d->e 가 되어 있어야 한다.

이문제에선 연결리스트의 헤드에 접근할 수 없다. 삭제할 노드에만 접근할 수 있다.
해법은 다음 노드의 데이터를 현재 노드에 복사한 다음에, 다음 노드를 지우는 것이다.
```
boolean deleteNode(LinkedListNode n){
    if(n == null || n.next == null){
        return false;
    }
    LinkedListNode next = n.next;
    n.data = next.data;
    n.next = next.next;
    return true;
}
```

삭제할 노드가 리스트의 마지막 노드인 경우에는 풀 수 없다.
마지막 노드인 경우 그냥 빈자리 노드라고 표시해두는 것도 한 방법이다.



