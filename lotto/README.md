
deep learning으로 로또 1등을 예측해보자
================================================
소스코드를 다운로드 하고
genNumbers.py를 실행 하시면 됩니다.

softmax classification deep learning 로또 1등을 예측해보자

<pre>
결과는 실패임 처음부터 이게 예측 가능 할 것이라고는 생각한 적 없음
다만 X에 대해서 Y를 예측하는 것을 해보기 위한 테스트 였음
하지만 그것 조차도 못함
왜냐면 기존 softmax classification 예제들은 Y쪽에 1이 하나였지만
[1, 0, 0, 0, 0, 0, 0, 0, 0, 0]
로또 같은 경우 내가 정의한 Y는 Y쪽에 1이 6개 였음
그래서 Y를 다시 softmax를 적용해서 전체 합을 1로 맞췄지만 역시 안됨 OTL
왜 안되는지 모르겠음


로또 데이터는 로또 site에서 excel로 다운로드 가능
http://www.nlotto.co.kr/gameResult.do?method=allWin
1회차 ~ 741회차 까지 다운로드 함


X, Y 데이터 정의
X는 회차 1~741, 1 feature로 설정
Y는 6개의 당첨 번호를 입력
[741회차 5, 21, 27, 34, 44, 45]
배열 45개를 잡아서 인덱스를 마킹함
[0, 0, 0, 0, 1, 0, 0, 0, 0, 0,
 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
 1, 0, 0, 0, 0, 0, 1, 0, 0, 0,
 0, 0, 0, 1, 0, 0, 0, 0, 0, 0,
 0, 0, 0, 1, 1]


genData.py
genData.py는 자연수로 된 당첨 번호를
45개 배열로 바꿔주는 프로그램
ori.data => input.data 작업을 해줌
ori.data, 741	5	21	27	34	44	45
input.data, [741, 0, 0, 0, 0, 1, ... ... 1, 1]


genNumber.py
로또 1등 예측을 해줌
(사실은 전혀 안되고 있음 ㅋㅋㅋ OTL)


genNumber.py 실행
일단 cost가 0.00~같이 작은 값으로 떨어져야 하지만
3.8 정도에서 머물러 버림
Epoch: 0001 cost= 378.201416016
Epoch: 0201 cost= 3.806513071
Epoch: 0401 cost= 3.806514978
Epoch: 0601 cost= 3.806518078
Epoch: 0801 cost= 3.806516171
Epoch: 1001 cost= 3.806515694
Epoch: 1201 cost= 3.806515694
Epoch: 1401 cost= 3.806515455
Epoch: 1601 cost= 3.806515694
Epoch: 1801 cost= 3.806516171
Epoch: 2001 cost= 3.806516171
Optimization Finished!


저 3.806516171 값은
페이스북 TensorFlowKR분들이 알려 줬는데
https://www.facebook.com/groups/TensorFlowKR/permalink/427913470883050/
-ln(1/45) 라고 함 근데 왜 저렇게 됬는지는 모름 ㅠㅠ
https://www.google.co.kr/webhp?sourceid=chrome-instant&ion=1&espv=2&ie=UTF-8#q=-ln(1/45)


나만 그런게 아님
http://qiita.com/yai/items/a128727ffdd334a4bc57
나 중에 찾게된 자료인데 이 친구도 3.7에서 cost가 안떨어짐


예측 데이터도 형편 없음
죄다 같은 값만 나옴 의미 없는 값이 나옴
[741]  27 20 34 40 1 17
[431]  27 20 34 40 1 17
[279]  27 20 34 40 1 17
[112]  27 20 34 40 1 17
[  1]  27 20 34 40 1 17


그래서 생각한 것이
X의 feature를 늘리면 어떨까 ㅋㅋㅋ
faeture를 12개로 증가 시킴
0741회 2017년 02월 11일 => 이 12개 숫자를 각각 feature로 잡음
[0, 7, 4, 1, 2, 0, 1, 7, 0, 2, 1, 1, ...


feature_genNumber.py 실행
전혀 의미 없음 cost 3.8에서 멈춤
Epoch: 0001 cost= 41.969722748
Epoch: 0201 cost= 3.806516171
Epoch: 0401 cost= 3.806515694
Epoch: 0601 cost= 3.806515455
Epoch: 0801 cost= 3.806516647
Epoch: 1001 cost= 3.806515694
Epoch: 1201 cost= 3.806516171
Epoch: 1401 cost= 3.806516171
Epoch: 1601 cost= 3.806516171
Epoch: 1801 cost= 3.806516171
Epoch: 2001 cost= 3.806515694
Optimization Finished!


의미 없는 예측값 나옴 왠지를 모름 ㅋㅋ
[0, 7, 4, 1, 2, 0, 1, 7, 0, 2, 1, 1]  27 20 34 34 1 17
[0, 7, 1, 0, 2, 0, 1, 6, 0, 7, 0, 9]  27 20 34 34 1 17
[0, 6, 6, 9, 2, 0, 1, 5, 0, 9, 2, 6]  27 20 34 34 1 17
[0, 5, 9, 4, 2, 0, 1, 4, 0, 4, 1, 9]  27 20 34 34 1 17
[0, 5, 1, 7, 2, 0, 1, 2, 1, 0, 2, 7]  27 20 34 34 1 17
[0, 0, 0, 1, 2, 0, 0, 2, 1, 2, 0, 7]  27 20 34 34 1 17
</pre>
