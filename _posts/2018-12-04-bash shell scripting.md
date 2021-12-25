---
title: bash shell scripting
author: Alan
layout: post
tags:
- 코딩
- coding
- shell programming
- work
- bash
- git
- shortlog
---

오늘 CODEOWNERS에서 아직 gitlab으로 이동하지 않은 캡슐들에 대해 이전통보를 보조하는 임무를 맡았다.
각 폴더별로 담당자를 찾아서 알려드려야 했는 데, github에서 history 버튼 누르고 일일이 확인하자니..
눈도 아프고.ㅠㅠ... 누가 중복이었는지도 모르겠고.. github 이름만 알려드리면 되는건지?

 누가 담당자인지 그리고 어떻게 연락해야하는지가 오리무중이었다.

그 때 사수 Aaron님께서 script 짜는 것을 추천해주셨다. :)
근데 아는 게 없어서 업혀가며 배우게 된 bash script!

```
//주석은 어떻게 할까.. 아직 모르겠네
VAR=`pwd` //변수는 타입없이 선언
echo $VAR //호출시 변수는 $를 앞에 붙이고, 왠만한 명령어는 그냥 치면 된다.

while read LINE do
...
echo $LINE > result.txt
done < $1 //이렇게 $1 등의 숫자표현은 이 파일을 호출할때 주는 인자다.
```

음 코드가 모두 기억나지는 않지만 이런식으로 파일을 한줄 읽어서 표시하기로 했고
`git shortlog` 명령어도 배웠다.

git 책에서 보던 pretty 함수를 쓸랬는데 shortlog는 커밋 개수로 나오고,
무엇보다 중복되는 계정이 안나온다. (log는 commit별로 나오기때문에..)

그리고 `sort -u`, `xargs` 도 배웠다.
`sort -u`: 끝에 파이프라인 후 사용하면 결과를 정리해준다.
`xargs`  : 파이프라인 후 사용하면 결과를 다른 명령의 입력으로 사용할 수 있다.

오늘도 알차게 배우고 일해서 너무 즐거운 것.. :)

--------------------------------------------------------------------------------------------------
\[en\]
Today, I got a mission about supporting an moving announcement to the capsules(in bixby2.0) which are not moved to gitLab yet.
To do that, I need to find who are contributing to each folders.
I was finding the people with using `history` button in Github.
But it was so inefficient.

 My senior Aaron told me to script this work.
I haven't made any shell script files but he show me how to do this and it worked! (most of the code was from him T.T)

the code was like..
```
//I don't know how to comment in script yet.
VAR=`pwd` //variables are defined without type casting.
echo $VAR //if you wanna use variables, you have to put `$` in front of the name of the variable.
          //and you can use any of commands in bash. (use exec if you can't)
while read LINE do
...
echo $LINE > result.txt
done < $1 //these $number expressions are for arguments when i execute this shell script.
```

And i've also learned about `sort -u`, `xargs`.

It was so joyful day that i learned much and worked well.