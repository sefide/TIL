API나 이벤트에서 전달하고자 하는 정보를 추가할 때, 기존에 전달하고 있던 메세지의 명확성을 헤치지 않는지 고민을 해볼 필요가 있다.

<br> 

예를 들면 아래와 같은 정보를 전달하는 메세지가 기존에 있었다고 가정해보자.

- AS-IS
```
data : {
    id: 1,
    quantity: 5
}
```

<br>

아래 **TO-BE** 와 같이 quantity에 대한 info가 필요하여 info_list를 추가하게 되는 경우가 생길 수 있다.

- TO-BE (info_list를 추가)
```
data : {
    id: 1,
    quantity: 5,
    info_list: {
      [
        info_id: 1001,
        info_type: "FRIEND",
        quantity: 2
      ],
      [
        info_id: 1002,
        info_type: "FAMILY",
        quantity: 3
      ]
    }
}
```

<br>
<br>

상황에 따라서는 상위 레벨의 quantity의 모든 수량에 대해 info 가 제공되지 않을 수 있다. 그럼 아래와 같은 메세지를 처음 본 사람이라면 'quantity가 5개인데 info_list의 quantity 합산은 왜 4개지?' 라는 의문이 들 수 있다.

```
data : {
    id: 1,
    quantity: 5,
    info_list: {
      [
        info_id: 1001,
        info_type: "FRIEND",
        quantity: 2
      ],
      [
        info_id: 1002,
        info_type: "FAMILY",
        quantity: 2 -- 2개인 경우
      ]
    }
}
```


<br>
<br>

이럴 땐 **TO-BE2** 와 같이 메세지를 분리하는 것도 좋은 방법이다.

- TO-BE2 (3개의 메세지로 분리)
```
-- 1번 메세지
data : {
    id: 1,
    quantity: 3,
    info: {
      id: 1001,
      type: "FRIEND"
    }
}

-- 2번 메세지
data : {
    id: 2,
    quantity: 3,
    info: {
      id: 1002,
      type: "FAMILY"
    }
}

-- 3번 메세지
data : {
    id: 3,
    quantity: 1,
    info: null
}
```
