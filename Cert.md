
# acme 로 인증서 발급

```
# 우분투 기준임.
# 필수 라이브러리 설치
apt install -fy socat
curl -L get.acme.sh | bash

# 기 발급된 도메인 인증서를 갱신 함 (Crontab 내용임)
# - AWS Route53기준
DOMAIN="foo.com"
/root/.acme.sh/acme.sh \
  --renew --dns dns_aws \
  -d ${DOMAIN} -d *.${DOMAIN} \
  -d *.bar.${DOMAIN} \
  --keylength 2048

# crt 파일 생성.
cat /root/.acme.sh/${DOMAIN}/fullchain.cer > ${DOMAIN}.crt
cat /root/.acme.sh/${DOMAIN}/fullchain.cer /root/.acme.sh/${DOMAIN}/${DOMAIN}.key  > ${DOMAIN}.pem
cat /root/.acme.sh/${DOMAIN}/${DOMAIN}.key  > ${DOMAIN}.key

#인증서 발급
/root/.acme.sh/acme.sh \
  --issue --dns dns_aws \
  -d ${DOMAIN} -d *.${DOMAIN} \
  -d *.bar.${DOMAIN} \
  --keylength 2048
# EC-256알고리즘을 사용 할수도 있음
#  --keylength ec-256
```