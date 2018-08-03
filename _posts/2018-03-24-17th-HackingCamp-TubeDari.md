---
layout: post
title: 17th HackingCamp - TubeDari
subtitle: 17th HackingCamp - TubeDari
excerpt: 7th HackingCamp - TubeDari 출제자가 작성하는 풀이입니다.
tags:
  - writeup
  - forensic
  - steganography
published: true
---



> 여하!     (여러분 하이! 라는 뜻 ㅎ)

`문제명: TubeDari ` / `타입: forensic, steganography`

HackingCamp 17th CTF "TubeDari" writeup  

![문제정보1](https://demonteam.org/content/images/2018/03/-----------2018-03-09------6.09.02.png)

![문제정보2](https://demonteam.org/content/images/2018/03/-----------2018-03-24------3.59.26.png)
fileDownload -> [tubedari.pcapng](http://http://hun.jaewon.me/index.php/s/TgLGTHmT3OLnXai/download)
MD5 hash : dcb636c0d173d92f56d02a4cd8ebf020

![파일포맷체크](https://demonteam.org/content/images/2018/03/-----------2018-03-24------4.04.59.png)
주어진 파일은 pcap-ng포맷의 파일입니다.  
pcapng 포맷은 기존 pcap 포맷보다 더 많은 정보들을 포함하고 있어 기존 pcap 파일보다 사이즈가 큽니다.  
다음에 네트워크 패킷을 분석해야 하는 경우가 생길 때 pcapng 파일이 주어지면 더 얻을 정보가 많아지기에 분석할 맛이 더 나겠죠!?  
이제 본격적으로 본론으로 고고!!  

네트워크 패킷 파일 같은 경우에는 툴을 이용해서 분석하는 게 통상적입니다.  

> 개인적으로 wireshark외에 패킷 분석 도구를 하나 추천해드리자면,  
[Network miner  ](http://www.netresec.com/?page=NetworkMiner)와 같은 툴을 사용하시면 툴 내에서 데이터를 보다 더 보기 편리하게 가공해 보여줌으로 분석이 보다 더 편할 수 있습니다.  
​
제가 언급한 network miner와 같은 다른 패킷분석 도구는 추가적으로 포스팅해드리겠습니당.  
  

가장 대중적인 [wireshark](https://www.wireshark.org)라는 툴을 사용해 파일을 열어보겠습니다.  


![와샼으로 열어봄](https://demonteam.org/content/images/2018/03/-----------2018-03-24------4.04.24.png)

문제 내에서 이미지 파일이라는 것 밖에 알려주지 않아서...  
이미지 파일로 접근을 해야 될 것 같습니다.  

wireshark내에서 HTTP, SMB프로토콜등에서 주고받은 파일을 추출하기 위해서는  
File -> Export Objects -> 추출을 원하는 프로토콜을 이용하면 됩니다.  
  

![와샥2](https://demonteam.org/content/images/2018/03/-----------2018-03-24------4.22.40.png)

이러한 류의 쉬운 네트워크 포렌식 문제들은 대게 파일을 HTTP protocol로 전달받기에...  
export HTTP부터 가봅시다.  

![에이치티티피 오브젝트 리수투](https://demonteam.org/content/images/2018/03/-----------2018-03-24------4.27.32.png)

우측 하단에 Save All을 클릭해서 파일들을 다 저장해봅시다.  

![저장후](https://demonteam.org/content/images/2018/03/-----------2018-03-24------4.30.20.png)
음... 근데 이미지가 없어보이네요?  
어 근데 밑에 TubeDari-Mission이라는게 보이네요.  

![파일체크 ㅇㅋ](https://demonteam.org/content/images/2018/03/-----------2018-03-24------5.53.50.png)
HTML도큐멘트네요.

![ㄹㅇ굳](https://demonteam.org/content/images/2018/03/-----------2018-03-24------5.54.19.png)
열어보니 이미지가 여러 개 로딩하는 부분을 볼 수 있었습니다.  

*THIS IS image included MISSION MESSAGE.*  
(한국어로 친절하게 해석해드립니다. -> 이것은 미션 메세지가 포함된 이미지다!)  
라는 텍스트 위에 이미지가 미션 메시지가 포함된 이미지인듯합니다..  




![-----------2018-03-24------5.55.37](https://demonteam.org/content/images/2018/03/-----------2018-03-24------5.55.37.png)

html의 소스코드를 열어서 보니...  
http://cfile25.uf.tistory.com/image/995FEF4A5A62CEB403A8E6 에서 995FEF4A5A62CEB403A8E6라는 파일의 이미지를 불러오네요.  

![-----------2018-03-24------5.56.50](https://demonteam.org/content/images/2018/03/-----------2018-03-24------5.56.50.png)  
 
하지만 접속을 해보니 404 not found가 뜹니다. 이미지가 전산상에 지워진듯합니다.  
아까 export http로 추출해낸 데이터들 중에 995FEF4A5A62CEB403A8E6라는 파일명을 본것 같죠?  

![-----------2018-03-24------5.58.30](https://demonteam.org/content/images/2018/03/-----------2018-03-24------5.58.30.png)
빙고, 패킷내에 해당 파일이 있음을 확인했습니다.  

확장자도 없고 혹시 모르니깐 파일 시그니쳐를 확인해 보겠숩니다.  
![-----------2018-03-24------4.32.06](https://demonteam.org/content/images/2018/03/-----------2018-03-24------4.32.06.png)

땋 png 이미지군요!!  
파일을 image로 열어보면~~~  

![995FEF4A5A62CEB403A8E6](https://demonteam.org/content/images/2018/03/995FEF4A5A62CEB403A8E6.png)
TubeDari라는 국제 테러단체 마크가 보이네요.  
근데 영어로 steganography가 적혀있다.  

# *ㄸㅣ-용!? steganography가 뭐야?*  
> 스테가노그래피(Steganography)는 전달하려는 정보를 이미지 파일이나 MP3파일, 동영상 파일 등에 숨기는 기술의 일종입니다. [more...](https://en.wikipedia.org/wiki/Steganography)  

아... 그러면 위에 이미지에 정보가 숨겨있는 건가!?라는 것을 의심해 볼 수 있겠네요.  

스테가노그래피는 종류가 상당히 많습니다...  

# 스태가노그래피의 쉬운 예  
![원본](https://demonteam.org/content/images/2018/03/Test1.jpg)![EOI에 데이터 숨긴것](https://demonteam.org/content/images/2018/03/Test2.jpg)  

위 이미지가 같은 이미지일까요? 왼쪽 이미지가 원본이고, 오른쪽 이미지가 EOI를 이용해 데이터를 숨긴 파일입니다.  

여러 이미지 파일은 특정 바이트 값으로 시작하여 특정 바이트 값으로 끝납니다.  
특히 JPG 파일은 FF D8로 시작하여 FF D9로 끝납니다.  
그래서 JPG의 EOI(End of Image)는 FF D9가 되는데,,,  
EOI 뒤부터는 이미지가 이미 끝난 후이므로 이후 데이터는 무시되기에, 아무 데이터나 적어도 됩니다!  

가장 simple하죠?   

실제로 오른쪽 사진은 아래와 같이 데이터를 숨겼습니다.  
![-----------2018-03-24------4.58.32-1](https://demonteam.org/content/images/2018/03/-----------2018-03-24------4.58.32-1.png)



그 외에도 LSB(Least Significant Bit)라든지 다양한 스태가노그래피를 비롯해서 다양한 복잡한 형태의 알고리즘의 스태가노그래피도 있습니다. 자세한 기법과 도구를 찾아보고 싶으시면 [다음]  (http://www.jjtc.com/Steganography/tools.html)을 참고해주세요.  



# 바로 결론으로 가서...  
제가 문제를 만들당시 해당 이미지에 스태가노그래피를 적용할때 [Steganography online tool](http://stylesuxx.github.io/steganography/)를 사용했습니다.  

해당 이미지에 적용한 스테가노그래피의 알고리즘을 [개발자 깃헙](https://github.com/stylesuxx/steganography/blob/gh-pages/script.js)을 통해 참고해서 일부만 보면...  
![code](https://demonteam.org/content/images/2018/03/-----------2018-03-24------5.26.11.png)
다음과 같은식으로 데이터를 숨겨 putImageData를 하는것을 알수있습니다.  

![specs](https://demonteam.org/content/images/2018/03/-----------2018-03-24------5.23.16.png)  

상세한 정보는 [개발자의 깃헙](https://github.com/stylesuxx/steganography#steganography-online)에서 보시면됩니다.  


원래 풀 때는 위와 같은 과정을 똑같이 밟을 필요가 없지만...  
좀 길게 적어야 풀이가 멋지므로... 문제의 적혀진 스토리에 따라 길게길게 writeup 작성을 완료했습니다 >~<  

다음엔 제가 풀이 쓸 때도 재미있고 여러분이 풀 때도 재미있고,  
풀이를 볼 때 또한 재미난 신박하고 아주 재미나는 포렌식 문제로 찾아뵙겠습니다~~  
