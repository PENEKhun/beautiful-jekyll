---
layout: post
title: CTFzone 2018 quals - Stupid dispute
subtitle: CTFzone 2018 quals - Stupid dispute
excerpt: OSINT유형의 stupid dispute문제의 라이트업 입니다.
tags:
  - writeup
  - OSINT
published: true
---


### 문제정보
`문제명: stupid dispute ` / `타입: OSINT`
>So, @inf073chk4, our dispute is still on! I've promised to hack all your hidden services and, believe me, I'll do it! Flag is: ctfzone{md5(flag)}


### 문제접근 방식
1. @inf073chk4와 관련된 정보를 SNS검색
1.twitter, telegram, facebook ... and so on
2. 해당 계정에서 얻을 수 있는 정보가 무엇인지 확인한다
1.account information, posts ... and so on
3. 문제내용을 토대로 공개 정보를 사용한다

### 풀이

1. @inf073chk4를 트위터에 검색합니다  <https://twitter.com/inf073chk4>
이후 다음과 같은 정보를 볼 수 있습니다.

<img src="https://penekhun.github.io/img/WuIMG/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202018-07-28%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%202.07.42.png" alt="one" style="width: 200px;height:300px"/>

러시아어는 번역기를 돌려보았지만, 별 의미가 없음을 알았고,
shark09@mail.ru는 당장은 아니지만 언젠가 쓰일지도 모른다.

<img src="http://penekhun.github.io/img/WuIMG/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202018-07-28%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%202.07.37.png" alt="two" style="width: 500px;height:300px"/>



<br />
`0n3 d4y 7h3r3 w1ll b3 4 b3773r 53rv1c3 <https://bit.ly/2LxsBJF>`

`fuck 7h15 53rv1c3 17'5 4m4z1n6 <http://dumpedlqezarfife.onion.lu/>`

`h1 6uy5, 1'm b4ck!`

다음과 같이 최근에 직접 작성한 게시글(3개)를 발견 할 수 있었다.

<http://dumpelqezarfife.onion.lu>를 접속하면,
<img src="https://penekhun.github.io/img/WuIMG/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202018-07-28%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%202.39.56.png" alt="thr" style="width:700px; height:400px"/>

다음과 같은 password dumped사이트를 볼수있다,
아이디/이메일를 입력하면 해킹된 database를 통해 password를 불러오는 사이트였다.


우리는 아까 이메일정보를 간접적으로 획득 하였다.
`shark09@mail.ru`
이를 검색해보자.

<img src="http://penekhun.github.io/img/WuIMG/shark09mailru.png" alt="fr" style="width:500px;heigth:300px"/>

shark09@mail.ru에 해당하는 password가 fsdfdsfsd임을 알게되었다.

이어서 <https://bit.ly/2LxsBJF>를 접속하면,
<img src="http://penekhun.github.io/img/WuIMG/pp.png" alt="six" style="width:700px; height:400px"/>

>Hi, %username%! Let me share a piece of pie with you - here is a cool database, I still don't have such file in my collection. But the only problem here is that the file has not yet been parsed. I think I'll do it later.

>I suddenly had a desire to build my own database of leaked data and make my own service out of it. It is still a lot to be done, but I'm on the right way. It's important to make a good layout of the site and find a good hosting for storing our data, but it has to be in the tornet.

>Now i'm slowly uploading data to my SFTP server here and plan to move to my other onion site later, the link is in the title. Have a nice day and pay attention to every word - but next picture is for filling content only ;).


간단하게 한국말로 적으면,
>안녕, 엄청 대단한 데이터 베이스를 발견했지만 아직 파싱되지 않았어.
>나는 이러한 데이터 베이스를 가지고 나만의 서비스를 만들거야.
>나는 지금 SFTP서버에 올리고 있고, 천천히 onion사이트에 옮길거야.
>링크는 타이틀에 있어. 사진은 낚시니깐 별 상관 마!

일단 SFTP서버에 올리고 있다니깐 SFTP로 접속해보자.


```
$ sftp shark09@93.170.105.93
password : fsdfdsfsd
```
접속이 된다... 하지만

```
Connected to 93.170.105.93.
sftp> ls
remote readdir("/"): Permission denied
```


아무런 권한도 없다.
그래서 이번엔 ssh로 접속해보았다.
```
$ ssh shark09@93.170.105.93
password : fsdfdsfsd

Could not chdir to home directory /sftp/s00zaran: Permission denied
This service allows sftp connections only.
Connection to 93.170.105.93 closed.
```
s00zaran이라는 또다른 무언가가 있음을 암시해주고 connetion closed 되었다.

이후 onion사이트에 올린다고 했고, 그 링크는 타이틀에 있다고 했다.
```
view-source:http://93.170.105.93/

<html>
    <head>
        <title>dumpitl6ghazgb2n</title>
        <meta name="language" content="en">
        .....
```
타이틀이 dumpitl6ghazgb2n임을 확인했고, onion사이트 니깐
최종 링크는 `dumpitl6ghazgb2n.onion`이 되겠다.

이 링크로 sftp를 한번 접속해 보겠다.  
## onion사이트라서 일반적으로 sftp접속 하면 안된다.  
`tor &`  이후 `torify sftp ID@dumpitl6ghazgb2n.onion` 해주자.



```
$ torify sftp shark09@dumpitl6ghazgb2n.onion
password : fsdfdsfsd
Permission denied, please try again.
```
shark09로는 접속이 되지 않는다.
s00zaran계정으로 재 시도해보자.



```
$ torify sftp s00zaran@dumpitl6ghazgb2n.onion
password : fsdfdsfsd
Connected to s00zaran@dumpitl6ghazgb2n.onion.
sftp> 
```
연결이 되었다.
이를 계정으로 ssh에 접속해 보았다.

```
$ torify ssh s00zaran@dumpitl6ghazgb2n.onion
s00zaran@dumpitl6ghazgb2n.onion's password: 

Okey, okey, you dit it! You're in the right place...
Keep your data secure and N3v3R_G1V3_uP !
Good luck ;)

Connection to dumpitl6ghazgb2n.onion closed.
```


```
md5 -s "N3v3R_G1V3_uP"
MD5 ("N3v3R_G1V3_uP") = 23ce0e1d28059cae9c6cc1a3d10b611d
```



`ctfzone{23ce0e1d28059cae9c6cc1a3d10b611d}`
