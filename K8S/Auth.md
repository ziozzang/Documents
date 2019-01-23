# Token
## 생성
* API 등에 사용 함

```
TOKEN=$(kubectl describe secret $(kubectl get secrets | grep "^default" | cut -f1 -d ' ') | grep -E '^token' | cut -f2 -d':' | tr -d " ")

# 아래와 같이 헤더에 인증으로 사용 함
--header "Authorization: Bearer $TOKEN"
```


