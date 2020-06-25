---
layout  : wiki
title   : 배열과 문자열 
summary : Array_Interview
date    : 2018-07-02 06:39:28 +0900
updated : 2018-07-24 17:32:18 +0900
tags    : 
toc     : true
public  : false
parent  : 
latex   : false
---
* TOC
{:toc}


# page : 282

### 1.1 중복이 없는가: 문자열이 주어졌을 때 중복이 등장하는 알고리즘을 작성하라. 자료구졸르 추가로 사용하지 않고 풀수 있는 알고리즘 또한 고민

> 우선 ASCII 문자열인지 유니코드 문자열인지 확인
(여기서는 ASCII 문자열이라고 가정)


방법1. 문자집합에서 i 번째 문자가 배열 내에 존재하는지 표시하는 boolean 배열을 사용하는 것이다. 같은 원소에 두번 들어오면 false 반환
또한 문자열의 길이가 문자의 집합크기보다 큰 경우 바로 false를 반환해도 된다.

```
boolean inUniqueChars(String str){
    if(str.length() > 128) return false;
    boolean[] char_set = new boolean[128];
    for(int i = 0 ; i < str.length(); i++){
        int al = str.charAt(i);
        if(char_set[val]){
            return false;
        }
        char_set[val] = true;
    }
    return true;
}

시간복잡도 O(n), 공간복잡도 O(1)
```

자료구조를 추가로 사용할 수 없다면 다음과 같이 할 수도 잇다.
1. 문자열 내의 각 문자를 다른 모든 문자와 비교한다. O(n2)/O(1)이 소요
2. 입력 문자열을 수저해도 된다면, O(nlogn) 시간에 문자열을 정렬한 뒤 문자열을 처음부터 훑어 나가면서 입접한 문자가 동일한지 검사해볼 수도 있다.

### 1.2 순열 확인 : 문자열 두개가 주어졌을 때 이 둘이 서로 순열관계에 있는지 확인 메서드

주의) 대소문자를 구별해서 따져야하는지...공백도 문자로 취급할 것인가....


문자열의 길이가 다르면, ㅅ로 순열관계 일 수 없다.

방법1. 정렬하라: 문자열을 정렬하면 같은 결과가 나올것이다.
    ==> 최적은 아니지만, 깔끔하고 단순하며 이해하기 쉽다.
```
public String sort(String s){
    char[] content = s.toCharArray();
    java.util.Arrays.sort(content);
    return new String(content);
}
public boolean permutation(String s, String t){
    if(s.length() != t.length()){
        return false;
    }
    return sort(s).equals(sort(t));
}

```

방법2. 문자의 출현횟수가 같은지 검사하라.(ASCII 를 사용한다 가정)
```
boolean permutation(String s, String t){
    if(s.length() != t.length()){
        return false;
    }
    int[] letters = new int[128];
    Char[] s_array = s.toCharArray();
    for(char c: s_array){
        letters[c]++;
    }

    for(int i = 0; i < t.length; i++){
        int c = (int)t.charAt(i);
        letters[c]--;
        if(letter[c] < 0){
            return false; 
        }
    }
    return true;
}
```

### 1.3 URLify: 문자열에 들어있는 모든 공백을 %20으로 바꾸는 메서드 작성, 문자열의 최종 길이가 함꼐 주어진다고 가정 
예) " Mr John Smith", 13 -> "Mr%20John%20Smith"

문자열 조작문제를 풀때 문자열 뒤에서부터 거꾸로 편진해나가는 것이 보통이다.
마지막 부분에 여유공간을 만들어 유용하게 사용할 수 있기 때문이다.
```
void replaceSpaces(char[] str, int trueLength){
    int spaceCount = 0, index = 0, i = 0;
    for(i = 0 ; i < trueLength; i++){
        if(str[i] == ' '){
            spaceCount++;
        }
    }
    index =  trueLength + spaceCount *2;
    if(trueLength < str.length) str[trueLength] = '\0'; // 배열의 끝
    for(i = trueLength - 1; i > 0; i--){
        if(str[i] == ' '){
            str[index - 1] = '0';
            str[index - 2] = '2';
            str[index - 3] = '%';
        } else {
            str[index - 1]  = str[i];
            index--;
        }
    }
}
```
String 는 수정이 불가능하므로 문자열 배열을 사용했다.

### 1.4 회문 순열: 주어진 문자열이 회문순열인지 확인하는 함수 작성
 ==> 회문이란 앞으로 읽으나 뒤로 읽으나 같은 단어 혹은 구절
     순열이란 문자열을 재배치하는 것을 뜻한다.
예) 입력: tact cat, 출력 True( taco cat, atcocta 등 )

단 한개의 문자만 홀수 여야 한다. 
다시 말해, 문자열의 길이가 짝수일때는 모두 짝수, 홀수 일때는 하나만 홀수 

```
boolean isPermutationOfPalindrome(String phrase){

    int countOdd = 0;
    int[] table = new int[Character.getNumericValue('z') - Character.getNumericValue('a') + 1]
    for(char c : phrase.toCharArray()){
        int x =getCharNumber(c);
        if(x != -1){
            table[x]++;
            if(table[x] %2 == 1){
                countOdd++;
            }else{
                countOdd--;
            }
        }
    }

    return countOdd <= 1;
}

```
### 1.5 하나빼기: 문자열을 편집하는 방법에는 세가지 종류가 있다. 삽입, 삭제, 교체.
    두개 문자열이 주어졌을때, 문자열을 같게 하기 위한 편집 횟수가 1회 이내인지 확인하는 함수작성
예) pale, ple -> true, palse, pale ->true, pale, bale -> true, pale, bake -> false

각 연산이 무엇을 의미하는지 고민하면 도움이 된다.
- 교체 : 두 문자열에서 하나의 문자만 다르다는 뜻
- 삽입 : 문자열의 특정부분을 빈공간으로 남겨두면 그부분을 제외한 나머지 부분이 동일하다는 뜻
- 삭제 : 삽입의 반대 뜻

한 문자열에 대해서 삽입, 삭제, 교체 연산을 모두 적용하지 않아도, 문자열의 길이를 알고 있으면, 어떤 연산을 적용해야 하는지 알 수 있다.

```
boolean oneEditAway(String first, String second){
    if(first.length() == second.length()){
        return oneEditReplace(first, second);
    }else if(first.length() + 1 == second.length()){
        return oneEditInsert(first, second);
    }else if(first.length() - 1 == second.length()){
        return oneEditInsert(first, second);
    }
    return false;
}
boolean oneEditReplace(String s1, String s2){
    boolean foundDiffernce = false
    for(int i = 0 ; i < s1.length(); i++){
        if(s1.charAt(i) != s2.charAt(i)){
            if(foundDiffernce){
                return false 
            }
            foundDiffernce =  true;
        }
    }
    return true;
}

boolean oneEditInsert(String s1, String s2){
    int index1 = 0;
    int index2 = 0;
    
    while(index2 < s2.length() && index1 < s1.length()){
        if(s1.charAt(index1) != s2.charAt(index2)){
            if(index1 != index2){
                return false;
            }
            index2++;
        }else{
            index1++;
            index2++;
        }
    }
    return true;
}
```
```
boolean oneEditAway(String first, String second){
    // 길이 체크
    if(Math.abs(first.length() - second.length()) > 1){
        return false;
    }
    // 길이가 짧은 문자열과 긴 문자열 찾기 
    String s1 = first.lenght() < second.length() ? first : second ;
    String s2 = first.lenght() < second.length() ? second : first ;
    

    int index1 = 0;
    int index2 = 0;
    boolean foundDiffernce = false
    
    while(index2 < s2.length() && index1 < s1.length()){
        if(s1.charAt(index1) != s2.charAt(index2)){
            // 반드시 첫번째로 다른 문자여야 한다.
            if(foundDiffernce) return false;
            foundDiffernce = true;
        
            if(s1.length() == s2.length()){
                index1++;
            }
        }else{
            index2++;
        }
    }
    return true;
}
```

### 1.6 문자열 압축: 반복되는 문자의 개수를 세는 방식의 기본적인 문자열 압축메서드를 작성하라.
예) aabccccaaa -> a2b1c5a3
만약 압축된 문자열의 길이가 기본문자열 보다 길다면 기존의 문자열을 반환해야한다.
문자열은 대소문자 알파벳(a-z) 으로만 이루어져 있다.

기초 방식
```
String compressBad(String str){
    String compressedString = "";
    int countConsecutive = 0;
    for(int i = 0; i < str.length() ; i++){
        countConsecutive++;
        // 다음 문자와 현재 문자가 같지 않다면 현재 문자열 결과 문자열에 추가해준다.
        if( i + 1 >== str.length() || str.charAt(i) != str.charAt(i+1)){
            compressedString +=""+ str.charAt(i) + countConsecutive;
            countConsecutive = 0;
        }
    }
    return compressedString.length() < str.length() ? compressedString : str;
}
```

```
String compressBad(String str){
    // 압축된 문자열의 길이가 입력 문자열보다 길다면 입력 문자열을 반환한다.
    int finalLength = countCompression(str);
    if(finalLength >= str.length()) return str;

    StringBuilder compressed = new StringBuilder(finalLength); // 처음크기
    int countConsecutive = 0;



    for(int i = 0; i < str.length() ; i++){
        countConsecutive++;
        // 다음 문자와 현재 문자가 같지 않다면 현재 문자열 결과 문자열에 추가해준다.
        if( i + 1 >== str.length() || str.charAt(i) != str.charAt(i+1)){
            compressed.append(str.charAt(i));
            compressed.append(countConsecutive);
            countConsecutive = 0;
        }
    }
    return compressed.toString();
}

int countCompression(String str){
    int compressedLength = 0;
    int countConsecutive = 0;
    for(int i = 0 ; i < str.length() ; i++){
        countConsecutive++;
        // 다음 문자와 현재 문자가 같지 않다면 길이를 증가시킨다.
        if( i + 1 >== str.length() || str.charAt(i) != str.charAt(i+1)){
            compressedString +=""+ String.valueOf(countConsecutive).length();
            countConsecutive = 0;
        }
    }
    return compressedLength;
}
```
이 방법의 장점 중 하나는 StringBuilder의 크기를 미리 설정할 수 있다는 점이다.
크기를 미리 설정하지 않은 경우 StringBuilder 는 내부적으로 설정한 크기를 넘어갔을 때, 전체 크기를 두배로 늘린다.
따라서, 최종 크기가 실제 필요로 하는 크기보다 두배 더 클 수도 있다.


### 1.7 행렬회전: 이미지를 표혀하는 NxN 행렬이 있다. 이미지의 각 픽셀은 4바이트로 표현. 이때 이미지를 90도 회전시키는 메서드 작성하라. 행렬을 추가로 사용하지 않고서도 할 수 있겠는가?
===> 가장 간단한 방법은 레이어별로 회전하도록 구현하는 것이다.
위쪽 모서리는 오른쪽으로, 오른쪽은 아래로, 아래는 왼쪽으로, 왼쪽은 위쪽으로,

위쪽 모서리를 배열에 복사한뒤 왼쪽 모서리를 맨위로 옮기고, 아래쪽 모서리는 왼쪽으로, ....
하지만 이 방법은 불필요하게 O(N) 만큼의 메모리가 추가로 든다.

이보다 더 나은방법은 교체작업을 인텍스별로 수행하는 것이다.

```
for i = 0 to n
    temp = top[i];
    top[i] = left[i];
    left[i] = bottom[i];
    bottom[i] = right[i];
    right[i] = temp;
```

```
boolean rotate(int[][] matrix){
    int(matrix.length == 0 || matrix.length != matrix[0].length) return false;
    int n = matrix.length;
    for(int layer = 0 ; layer < n / 2 ; layer++){
        int first = layer;
        int last = n - 1 - layer;
        for(int i = first ; i < last ; i++){
            int offset = i - first;
            int top = maxtrix[first][i]; // 위부분 저장
            // 왼쪽 -> 위쪽
            matrix[first][i] = matrix[fast-offset][first];
            // 아래쪽 -> 왼쪽
            matrix[fast-offset][first] = matrix[last][last-offset];
            // 오른쪽 -> 왼쪽
            matrix[last][last-offset] = matrix[i][last];
            // 위쪽 -> 아래쪽
            matrix[i][last] = top ;
        }
    }
    return true;
}
```

복잡도는 O(N^2) 인데, 적어도 N^2의 원소를 모두 건드려 봐야 하기 때문에 최선의 방법이다.

### 1.8 0행렬 : M x N 행렬의 한 원소가 0 일경우, 해당 원소가 속한 행과 열의 모든 원소를 0으로 설정하는 알고리즘을 작성하라

 ===> 얼핏 쉬워보인다. 행렬을 순회해 나가면서 값이 0인 셀을 발견하면 그 셀이 속한 행과 열을 모두 0으로 만든다.
0으로 바뀐 행이나 열의 또다른 셀을 나중에 방문하면, 그 셀이 속한 행과 열도 전부 0으로 만든다. 이러다 보면 전체가 0으로 바뀐다.

해결을 위해서 0의 위치를 기록할 행렬하나를 더둔다.
그 다음에 행렬을 다시 훑어 나가면서 0으로 바꾼다.
이방법은 O(MN) 만큰의 공간이 필요하다. 정말로 O(MN) 만큼 필요할까? 그렇지 않다.
같은 행과 열이 있는 값을 모두 0으로 만들 것이기 때문에 값이 0인 셀이 정확히 cell[2][4]에 있는지 알 필요가 없다. 
단지 두번째 어딘가에 있고 네번째 어딘가에 있다는 것만 알고 있으면 된다.

```
void setZeros(int[] matrix){
    boolean[] row = new boolean[matrix.length];
    boolean[] column = new boolean[matrix[0].length];
    // 값이 0인 행과 열의 인덱스를 저장한다.
    for(int i = 0; i< matrix.length; i++){
        for(int j = 0; j< matrix[0].length ; j++){
            if(matrix[i][j] == 0){
                row[i] = true;
                column[j] = true;
            }
        }
    }
    // 행의 원소를 전부 0으로 바꾼다.
    for(int i = 0 ; i < row.length ; i++){
        if(row[i]) nullifyRow(matrix, i);
    }
    // 열의 원소를 전부 0으로 바꾼다.
    for(int j = 0 ; j < matrix[0].length ; j++){
        if(column[j]) nullifyColumn(matrix, j);
    }
}

void nullifyRow(int[][] matrix, int row){
    for(int i = 0 ; i < matrix[0].length ; i++){
        matrix[row][i]= 0;
    }
}

void nullifyColumn(int[][] matrix, int col){
    for(int i = 0 ; i < matrix.length ; i++){
        matrix[i][col]= 0;
    }
}

```


### 1.9 문자열 회전 : 한단어가 다른 문자열에 포함되어있는지 판별하는 isSubstring 이라는 메서드가 있다고 하자.
                s1 과 s2 의 두 문자열이 주어지고, s2 가 s1 을 회전 시킨 결과린지 판별하고자 한다.

예를들어 waterbottle 은 --> erbottlewat 을회전시켜 얻을 수 있는 문자열이다.

isSubstring 메서드를 한 번만 호출해서 판별할 수 잇는 코드를 작성하라.


x = wat
y = erbottle
s2 = yx = erbottlewat

s1을 x와 y로 나눈뒤 xy는 s1이 되고, yx는 s2가 되는 방법이 있는지 찾으면 된다.

yx는 언제나xyxy의 부분문자열 이라는 사실을 알수 있다.
즉, s2 는 언제나 s1s1의 부분 문자열이 된다.

```
boolean isRotation(String s1, String s2){
    int len = s1.length();
    // s1 과 s2의 길이가 같고 빈 문자열이 아닌지 확인
    if(len ==s2.length && len > 0) {
        // s1 과 s2를 합친 결과를 새로운 버퍼에 저장한다.
        String s1s1 = s1 + s1;
        return isSubstring(s1s1, s2);
    }
    return false;
}
```
    
