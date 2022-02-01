---
layout: page
title: Portfolio
---




<p>&nbsp;</p><h5>[프로젝트 유형]</h5><h5>Personal Project&nbsp;&nbsp;​</h5><h5>&nbsp;</h5><p><span id="husky_bookmark_end_1631499966768"></span></p><h5>[배경 및 목표]</h5><div><span style="font-size: 11pt;">​</span><span style="font-size: 11pt;">&nbsp;최근 몇 년 동안 최신 CPU는 Meltdown-type의 공격을 겪고 있습니다. 이러한 공격은 사용자의 권한에서 수행되면 안되는 잘못된 Load 명령어의해 생성된 Transient instruction들을 악용하여 전달됩니다. 비밀 값은 일시적인 명령에 의해 Cache에 Encoding되며, 이는 차례로 Flush+Reload와 같은 Microarchitecture side channel attack에 의해 유출됩니다. 이러한 공격에 대한 최근 연구는 주로 새로운 취약한 </span><span style="font-size: 11pt;">Microarchitecture</span><span style="font-size: 11pt;">&nbsp;</span><span style="font-size: 11pt;">​unit을</span><span style="font-size: 11pt;">&nbsp;찾는 데 중점을 두고 있고, Transient execution window에서 실행할 수 있는 최대 transient instrucion의 수에 대한 연구는 관심이 적습니다.&nbsp;</span><span style="font-size: 11pt;">이 연구를 통한 시스템의 목표는 최대 transient execution window의 실행가능한 transient instruction들의 갯수를 정량적으로 다양한 시스템적 환경에서 알아내는 것을 목표로 하였습니다.</span></div><div><h5><br></h5><h5>[기술 스택]</h5><h5>Python C Mips bash gdb gdb-peda linux-kernel Git​<span style="font-size: 12px;">&nbsp;</span></h5><h5><span id="husky_bookmark_end_1630956886358"></span></h5><h5><br></h5><h5>[설계 및 과정]</h5><div><span style="font-size: 11pt;">2018년도 Meltdown의 저자가 공개한 PoC(Proof of Concept)중 physical memory를 dump하는 공격 PoC와 KASLR이 작동하는 환경에서, physical memory address의 시작주소를 파악하는 Breaking KASLR PoC를 재구성하여 Meltdown Attack을 활용한 최대 transient instruction의 갯수를 파악하는 PoC를 설계합니다. 또한 2019년 ZombieLoad의 저자가 공개한 PoC중 Variant 1을 활용하여 Linux환경에서 ZombieLoad를 활용한 최대 transient instruciton 갯수를 PoC를 설계합니다. Transient execution window를 의도적으로 생성시키고, 그 안에 최대한 많은 명령어를 수행시키면서 진행 상태에 대한 Log를 bash script를 통해 추출 후, python을 통해 시각화를 진행합니다.</span></div><div><br></div><h5>[결과물]</h5><h5>-&nbsp; "Scopus"&nbsp;<scopus><span style="font-size: 12px;">국제 학술대회 WISA</span>(The World Conference on Information Security Application)<span style="font-size: 12px;">​</span>​<span style="font-size: 12px;"> 구두</span><span id="husky_bookmark_end_1630956982990" style="font-size: 12px;"></span><span style="font-size: 12px;"> 발표&nbsp;</span><span id="husky_bookmark_end_1630956999330" style="font-size: 12px;"></span><span id="husky_bookmark_end_1630956991467" style="font-size: 12px;"></span><span style="font-size: 12px;">&nbsp;</span></scopus></h5><h5></h5><h5>- &nbsp;Springer(LNCS) 1저자 논문 게재​​(논문명:&nbsp;Quantitative Analysis on Attack Capacity in Meltdown-type Attacks​)</h5><p>&nbsp;</p><p>&nbsp;</p><p><img src="https://user-images.githubusercontent.com/67637935/132085898-1720f624-7924-4632-965b-7e492a25fa5f.png" alt="스크린샷 2021-09-04 오후 4 01 42" style="max-width: 100%;"></p>


<div class="attach"><span>영상첨부</span><div><a href="https://youtu.be/g7ykSmbfAsI" target="_blank">https://youtu.be/g7ykSmbfAsI</a></div><div><br></div></div></div>

<div class="attach"><span>학회링크</span><div><a href="https://wisa.or.kr/accepted" target="_blank">https://wisa.or.kr/accepted</a></div></div>


<p><br><br>

</p><h5></h5><h4>[산학연계 SW프로젝트]딥러닝 및 AUTO ML 기반 시계열 데이터 분석을 통한 일상 생활 패턴 분석 및 시스템 개발&nbsp;</h4><h5>​<span id="husky_bookmark_end_1630957380460"></span>
<span class="data">2020.06 - 2021.07</span>
<br></h5><h5><br></h5><h5><br></h5><h5>[프로젝트 유형]</h5><h5>Team Project (팀명: SAILB)&nbsp;</h5><div><br></div><div></div><h5>[관련 과목]</h5><h5>소프트웨어프로젝트 2, 산학협력캡스톤설계1</h5><div><br></div><div></div><h5>[배경 및 목표]</h5><h5><span style="font-size: 11pt; font-weight: normal;">​수면 단계 분석은 수면 장애를 겪고 있는 사람들에게 있어서 첫번째로 풀어야할 문제이지만, 현재까지 의사가 직접 수면 단계를 추정하는 것이 가장 최선의 접근 방법이기에 문제점이 있습니다. 이를 해결하기 위해서 상용화 할 수 있는 수면 예측 모델을 AUTO ML 환경을 바탕으로 딥러닝을 통해 구성하는 것을 목표로 하고 있습니다.</span><br></h5><div><h5><br></h5></div><h5>[기술 스택]</h5><h5>Python Tensorflow Pytorch Matlab PyQt Git</h5><h5><br></h5><div><h5>[설계 및 과정]</h5></div><div><span style="font-size: 11pt;">리클라이너 장비를 통해 직접 뇌파를 측정하여 데이터를 생성하고 측정한 데이터를 Band Path Filter를 통한 신호 분석 후 STFT를 통해 신호 분해법을 통해 전처리를 진행합니다. 그 이후, 멀티 채널 입력 데이터에 대한 Attention 알고리즘 </span><span style="font-family: MalgunGothic; font-size: 11pt;">CBAM (Convolutional Block Attention
Module)을 통한 특성간 연관분석을 통한 4가지 수면 상태에 대한 분류 모델을 설계합니다. 이 후, 강화학습을 통해 특징점을 검출하고 BOHB(Robust and Efficient Hyperparameter Optimization)과 AdamP를 통해 모델의 튜닝하고 최적화를 진행합니다. 최종적으로 만들어진 분류 데이터를 사용자에게 PyQt를 활용한 프로그램을 구성하여 상호작용합니다.</span><span style="font-size: 11pt;">​</span><span style="font-size: 11pt;">​</span></div>
		
	
	
		<div><span style="font-size: 11pt;"><br></span></div><div></div><h6>Architecture<img src="https://user-images.githubusercontent.com/67637935/131971407-e19f7c1b-8676-4562-bf6e-a85b4d3b2153.png" alt="image" style="font-size: 12px; max-width: 100%;"></h6><div>&nbsp;​<br></div>
<h5><br></h5><h5>[결과물]</h5><h5>- 제5회 산학연계 SW프로젝트 대상(총장상)</h5><h5>- 대한전자공학회 하계학술대회​ 게제 (논문명: EEG 단체널 기반 Inception 네트워크 순환 신경망을 결합한 자동 수면 단계 분류기)<span id="husky_bookmark_end_1630740390525"></span></h5><h5></h5><h5><br></h5><p style="margin-left: 0px;"><span style="font-size: 8.04px;"><b>Layout, Paper&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</b></span></p><p style="margin-left: 40px;">&nbsp;</p><p>​<img src="https://user-images.githubusercontent.com/67637935/132086417-42982ec3-0f0b-405b-8eae-823692f72d27.png" alt="스크린샷 2021-09-04 오후 4 20 29" style="max-width: 100%;"></p><p>&nbsp;</p><p>&nbsp;</p>


<div class="attach"><span>영상첨부</span><div><a href="https://www.youtube.com/watch?v=4lXywrhhnxE" target="_blank"> https://www.youtube.com/watch?v=4lXywrhhnxE</a></div><div><br></div></div>

<div class="attach"><span>프로젝트 링크</span><div><a href="https://github.com/Lukious/SAILB_project" target="_blank">https://github.com/Lukious/SAILB_project</a></div></div><div><br></div>


<p><br><br>

</p><h4>메일 통신 프로토콜 MITM 공격 취약성 분석 연구 (진행중) </h4><p>

<span class="data">2021.04 - 2021.11</span>

</p><h5><span style="font-size: 12px;"><br></span></h5><div><span style="font-size: 12px;"></span></div><h5>[프로젝트 유형]</h5><h5>Team Project</h5><div><br></div><div></div><h5><br></h5><div></div><h5>[배경 및 목표]<span style="font-weight: normal;">​</span></h5><h5><span style="font-size: 11pt; font-weight: normal;">&nbsp;Post 코로나 시대로 언택트 환경에서 업무를 진행하는 과정속에 우리는 살고 있습니다. 개인 간의 연락책으로는 kakaotalk, Line, SMS등의 IT 서비스를 이용하지만&nbsp; 기업과 기업, 기업과 개인의 연락수단으로 이메일을 통한 업무지시 및 결재 등 많은 통신이 이루어집니다. 그렇기에 이메일 통신의 보안은 더 강화되어야 합니다. 현재 이메일에 대한 실태를 조사하고, 연구를 목표로 프로젝트를 진행합니다.&nbsp;</span></h5><div><h5><br></h5></div><h5>[기술 스택]</h5><h5>C Python Email-Protocol(IMAP,SMTP.STARTTLS,DANE,MTA-STS) SMTP-api​</h5><h5><br></h5><div><br></div><div><h5>[설계 및 과정]</h5></div><div></div><h5><span style="font-weight: normal;"><span style="font-size: 11pt;">&nbsp;</span><span style="font-size: 11pt;">실제 네트워크 환경이 아닌 폐쇠된 네트워크 환경에서 이메일 서비스의 동작을 실증합니다. 폐쇠된 네트워크 환경을 만들기위해 vmware workstation에 MTA,MUA,DNS,ROUTER를 설치합니다. 그 이후, 전송되는 모든 패킷을 관장하는 router에 공격자가 점거하였다는 가정하에 이메일 보안과 관련된 다양한 프로토콜들을 실증합니다. 그리고 현재 알려져있는 공격들을 Router환경에 맞게 적용시켜, 추가적인 취약점이 있는지 분석합니다.</span></span></h5><div><img src="https://user-images.githubusercontent.com/67637935/133348151-4ac3c056-364f-4ac5-a857-16b97a4a810f.png" alt="스크린샷 2021-09-15 오전 8 44 37" style="max-width: 100%;"></div><div><span style="font-size: 14.6667px;">Concept of study (IMG ref:&nbsp;</span><span style="font-size: 10.5pt; font-family: Arial;">K</span><span style="font-size: 10.5pt; font-family: Arial;">ambourakis</span><span style="font-size: 10.5pt; font-family: Arial;">&nbsp;et al., IEEE Access 2020)</span>​</div><div><br></div><h5>[결과물(진행중)]</h5><h5>-&nbsp; 국제 저널 작성 예정</h5><p>&nbsp;</p>


<p><br><br>


</p><h4>Real-Time(실시간성) Cache Attack 공격 탐지 알림 Framework 개발 </h4><p>

<span class="data">2020.09 - 2020.12</span></p><p><span class="data"></span></p><p>&nbsp;</p><h5>[프로젝트 유형]</h5><h5>Team Project</h5><div><h5><br></h5></div><h5>[기술 스택]</h5><h5>Python pandas Git kakao-api solapi ​</h5><div><h5></h5>​<div><br>

<h5>[배경 및 설계]</h5><p>
<span style="font-size: 11pt;">실시간으로 공격을 탐지하는 모듈이 기존에 존재할 때, 탐지 후 사용자에게 얼마나 빠른 시간내에 실시간으로 탐지에 대한 알림을 보내줄 수 있는지 또한 큰 문제상황중 하나 입니다. 이를 해결하기 위해, 머신러닝을 활용해 cache attack을 실시간으로 탐지하는 모듈이 통해 influxdb로 cpu 코어별 cache access latency 값이 실시간으로 전송하게 됩니다. 그 이후, 실시간으로 전송된 값들 중 공격이라고 판단되는 임계값을 넘게된다면 client와 가까운 networking 기기(서비스: kakaotalk, slack, email)로 다양한 python api들을 통해 알림을 보내주는 Framework를 개발했습니다. 또한 influxdb와 grafana의 연동성을 통해 데이터를 시각화하는 과정 또한 진행하였습니다.</span></p><p><span style="font-size: 11pt;">&nbsp;</span></p><p><span style="font-size: 11pt;">&nbsp;</span></p><p><span style="font-size: 11pt;"></span></p><h5>[결과물]</h5><div><h5>- 소프트웨어 저작권 등록​</h5></div><p><img src="https://user-images.githubusercontent.com/67637935/132118684-43170c39-2342-4598-a6dc-a6ae9e6d8ee2.png" alt="스크린샷 2021-09-05 오후 4 12 04" style="max-width: 100%;"></p><p>&nbsp;</p><p>&nbsp;</p><p><br><br>




</p><p><br><br>



</p><h4>[광운대학교 2020년도 하계 단기 인턴십] Window PC의 악성 행위 탐지 및 모니터링 시스템 개발&nbsp;</h4><p>
<span class="data">2020.07 - 2020.08</span></p><h5><br></h5><h5>[프로젝트 유형]</h5></div><h5>Personal Project&nbsp;&nbsp;(회사명: InTheForest)​</h5><div><div><h5><br></h5></div><h5>[기술 스택]</h5></div><h5>Python PyQt Elasticsearch Logstash Kibana Bash xml winlogbeat Git​ viroustotal-api</h5><div><div>&nbsp;</div><div><h5>[배경 및 설계]</h5><p>사용자가 윈도우 환경에서 작업을 하고 있을 때가 아닌, 해커들은 사용자가 컴퓨터를 직접 사용하고 있지 않을 때 침입이 발생합니다. 침입 후 공격자는 다양한 악성행위를 이어갈텐데 이 것들을 방지하기 위해 반복적으로 모니터링할 수 있는 프로그램이 상용적으로 필요합니다. 그래서 저는 Sysmon-ELK를 활용해 시스템 개발을 진행하였습니다. 클라이언트(window) 환경에서 발생하는 Event들(Sysmon 로그)을 winlogbeat와 logstash를 통해 서버(ubuntu)로 송신합니다. 그 이후, Elasticsearch에서 데이터를 정제하고 시계열 형태로 정제된 데이터들은 Kibana와 PyQt를 통해 실시간으로 모니터링 및 데이터 시각화(분석)할 수 있는 시스템을 개발하였습니다.</p><p><br>

</p><h6> Architecture</h6><h6><img src="https://github.com/leesk212/Sysmon-EL-Python_PyQt/raw/master/img/Architecture.png" alt="ex_screenshot" style="font-size: 12px; max-width: 100%;"></h6><h6><br></h6>

<h6> Layout </h6><div><img src="https://github.com/leesk212/Sysmon-EL-Python_PyQt/raw/master/img/layout.png" alt="ex_screenshot" style="max-width: 100%;"></div>

<div class="attach"><span>프로젝트 링크 </span><div><a href=" https://github.com/leesk212/Sysmon-EL-Python_PyQt/" target="_blank"> 
https://github.com/leesk212/Sysmon-EL-Python_PyQt/</a></div></div>

<p><br><br>

</p><p>&nbsp;</p><h4>머신러닝과 SVM을 활용한 Intel CPU 취약점 공격 탐지 시스템 개발&nbsp;</h4><p>
<span class="data">2021.03 - 2021.05</span>&nbsp;</p><h5><br></h5><h5>[프로젝트 유형]</h5><h5>Personal Project ​</h5><h5></h5><div></div><h5><br></h5><h5>[관련 과목]</h5><h5>머신러닝​&nbsp;</h5><div><br></div><div></div><h5>[기술 스택]</h5><h5>Python Tensorflow HPC(pcm) Bash Git​</h5><div><br></div><h5>[배경 및 설계]</h5><div>공격자가 피해자 컴퓨터의 비밀 정보를 탈취하는 방법에는 다양한 방법이 있습니다. 그 중 인텔 CPU의 취약점을 악용한 공격들이 치명적이며 이를 방어 및 예방하기 위해서는 공격들의 기저가 되는 Micro-Architecture Attack (Flush+Reload, Flush+Flush, Meltdown)을 탐지할 수 있어야 합니다. 인텔 CPU의 취약점을 목표로 만들어진 공격들이 피해자 컴퓨터 에서 실행될 때 HPC(Hardware performance counter) 상태에 영향을 주며 이는 PCM (Processesor Counter Monitor)을 통해 HPC상태 변화를 확인할 수 있습니다. PCM으로부터 추출할 수 있고, 기존의 공격 탐지 논문에서 Feature로 사용한 Cache, IPC뿐만이 아니라, Temp, Power등 추가적인 Feature들로 다항식 커널 SVM(Support Vector Machine) 알고리즘을 활용해 기계 학습을 진행합니다. 학습 된 모델로 피해자의 컴퓨터가 어떤 Micro-Architecture Attack을 받고 있는지와 평상시 상태를 구분하는 과정을 통해 공격을 예방할 수 있는 모델을 만들어 공격 탐지를 진행합니다.&nbsp;&nbsp;</div><div><br></div><div><span style="font-size: 8.04px; font-weight: 700;">Architecture</span>​</div><div><img src="https://user-images.githubusercontent.com/67637935/131985282-2bd4c125-22f3-4b02-b852-005c5087df40.png" alt="2021-09-03_18-40-26" style="max-width: 100%;"></div><div><br></div><div><br></div>

<div class="attach"><span>프로젝트 링크 </span><div><a href="https://github.com/leesk212/Micro-Architecture-Attack-Detection-Model-using-Machine-Learning" target="_blank">https://github.com/leesk212/Micro-Architecture-Attack-Detection-Model-using-Machine-Learning</a></div></div>

<p><br><br>



</p><h4>RTOS 동작방식을 활용한 Mail-Service Concept 개발&nbsp;</h4><h4></h4><p><span class="data">2021.04 - 2021.06</span></p><p><span class="data"></span></p><h5><br></h5><h5>[프로젝트 유형]</h5></div><h5>Personal Project</h5><div><h5><br></h5><div></div><h5>[관련 과목]</h5><h5>임베디드시스템S/W설계​&nbsp;</h5><div><br></div><div></div><h5><span style="font-size: 12px;"></span>[기술 스택]​</h5><h5>C Git​</h5><div><br></div><h5>[배경 및 설계]</h5><p>&nbsp;본 프로젝트의 목표는 간단한 Mail-Service의 Topology 중 각각의 필수 기능을 하는 서버들에서 E-mail Packet의 이동과정을 RTOS를 통해 시각적 시뮬레이션으로 보여주고, 이를 통해 Mail-Service 각각의 서버들의 동작이 RTOS의 상태가 되어야지만 정상적인 service를 시행할 수 있음을 보여줍니다.&nbsp;</p><p>&nbsp;</p><p>구현한 프로젝트에서는 Clinet-A에서 Client-B로 메일을 전달하는 과정을 시각적으로 보여주고 있으며 그 사이에 Server-A, Router, DNS, Server-B가 있습니다. 전달을 위한 Packet의 이동 과정은 다음 5가지 과정을 통해 진행됩니다.&nbsp;</p><p>- 1) Client-A to Server-A (Prcoess of SMTP) : Client-A에서 ThunderBird와 같은 email application을 사용해 메일 전달을 담당하는 SMTP-Server 역할을 하는 Server-A로 패킷을 전달합니다.&nbsp;</p><p>- 2) Server-A to Router to DNS: SMTP 서버에서 MTA는 수신자의 도메인 확인하기 위해 DNS-Server를 찾습니다.</p><p>- 3) DNS to Router to Server-A: DNS 서버에 MX(Mail eXchanger) 서버 주소를 질의해서 찾습니다.&nbsp;</p><p>- 4) Server-A to Router to Server-B: 질의해서 수신자 서버를 찾으면 해당 서버로 메일을 SMTP 프로토콜을 통해 수신자의 Server(Server-B)로 패킷을 전달한다. 메일은 수신자의 서버 MTA로 도착합니다.</p><p>- 5) Server-B to Client-B (Process of PoP3 of IMAP): 도착한 메일은 MDA를 통해 수신자의 메일함에 메일을 넣습니다.받는 사용자는 본인의 MUA에서 원하는 프로토콜에 따라 POP또는 IMAP으로 메일을 받습니다.&nbsp;</p><p>&nbsp;</p><p>&nbsp;이 5단계 과정을 다음의 그림 1로 표현하였고, 이 후 그림 2와 같이 RTOS로 시각화 시뮬레이션을 진행하였습니다. RTOS에서는 연속해서 반복적으로 Clinet-A에서 Clinet-B로 메일을 보내는 과정을 동적으로 보여줌을 통해 이메일 시스템의 컨셉을 보여주었습니다.</p><p><img src="https://user-images.githubusercontent.com/67637935/132102103-edacd68f-dfdc-4b3e-a29d-4b305a3157d6.png" alt="스크린샷 2021-09-05 오전 1 45 24" style="max-width: 100%;"></p><p>&nbsp;</p>

<p><br><br>



</p></div></div>


<p>&nbsp;</p><h4>카카오 로그인 인증을 활용한 학생정보관리 서비스 개발</h4><p>
<span class="data">2020.09 - 2020.12</span>&nbsp;</p><h5><br></h5><div><h5><br></h5></div><div><h5>[프로젝트 유형]</h5><h5>Team Project (팀명: 디비만만)&nbsp; ​</h5><h5></h5><div></div><h5><br></h5><h5>[관련 과목]</h5><h5>데이터베이스및응용​&nbsp;</h5><div><br></div><div></div><div><h5>[기술 스택]</h5></div><h5>Python Flask Docker Vue(axios) Javascript Git</h5><div><br></div>

<h5>[소개]</h5><p>
학생정보관리 시스템을 주제로 DBMS를 연동하여 웹 애플리케이션을 개발하며 회원 가입 및 카카오 계정 연동, 개인 정보 조회, 강의 시간표 조회, 수강신청 기능, 과목별 공지사항 게시판, 학습결과(수강/성적 조회) 등의 기능들을 기본으로 포함합니다.</p><p>&nbsp;</p><h5>[설계 및 역할]​</h5><h6>[ My Backend Work ]</h6>

- 카카오데이터서버 연동 및 어플리케이션 개발, 개인code갱신기능구현, Access token을 통해 사용자 개인 정보 (email, 프로필 사진, 아이디, 성별 등) 갱신 기능 구현 <p>

- API (Flask 개발):  학생정보조회 및 검색 api 구현, 댓글관련 api 구현, 과목조회관련 api 구현</p><h6>[ My Frontend Work ]</h6>

-  Author Page 구현 <p>

- Login , FindPW, NewAccount Page 버튼 기능 연동 </p><p>

- 세션 유지 및 kakao 정보 쿠키화, Enrollment Page 버튼 기능 구현,과목 즐겨찾기 CRUD, 즐겨찾기 된 class 개인 등록 기능, Notice Page 버튼 기능 구현 </p><p>

- Axios: Login page 관련 kakao 데이터 센터 연동 구현, 중복 email 확인 기능 api 연동, Enrollment page 관련 class api 연동, Notice page 관련 post api 연동, Main page 관련 timetable api 연동 

</p><p>&nbsp;</p><p>&nbsp;</p><h6>Architecture</h6><h6><img src="https://user-images.githubusercontent.com/67637935/132084610-a00e56e3-b2ce-40e1-bb46-6856b4770f5c.png" alt="스크린샷 2021-09-04 오후 3 04 37" style="max-width: 100%;"></h6><h6><br></h6>



<div class="attach"><span>프로젝트 링크 </span><div><a href="https://github.com/0xF4D3C0D3/kw-db-project-2020" target="_blank">https://github.com/0xF4D3C0D3/kw-db-project-2020</a></div><div><br></div></div><div class="attach"><span>영상첨부</span><div><a href="https://www.youtube.com/watch?v=4eEvMKFw9_g&amp;t=332s" target="_blank">https://www.youtube.com/watch?v=4eEvMKFw9_g&amp;t=332s</a></div><div><br></div><div><br></div></div><br>



<p><br><br>
</p></div>


<p>&nbsp;</p><h4>FTP 서버 개발</h4><p>
<span class="data">2020.04 - 2020.06</span>


</p><h5><span style="font-size: 12px;"></span></h5><h5><br></h5><h5>[프로젝트 유형]</h5><h5>Personal Project</h5><div></div><h5><br></h5><h5>[관련 과목]</h5><h5>-&nbsp; 시스템프로그래밍, 시스템프로그래밍실습</h5><p>&nbsp;</p><p>&nbsp;</p><h5>[기술 스택]<span style="font-size: 12px;"></span></h5><h5>C linux-kernel bash</h5><h5>​</h5><p>&nbsp;</p><h5>[설계 및 과정]</h5><div>리눅스 환경에서 기본적인 리눅스 쉘 명령어들을 익히고 직접 내장 함수를 통해 구현해보는 과정을 수행하는 프로젝트를 진행 하였습니다. 또한 이를 통해 기본적인 client와 server의 통신을 위한 socket programming을 수행하였습니다. 그리고 내장 함수들을 통해 기본적인 명령어(ls -[a][l], pwd, cd, mkdir, rename, 등)​를&nbsp;수행하는 ftp server를 만들었습니다.&nbsp; 그 이후 fork() 내장 함수를 사용하여, 다중연결이 가능한 ftp server를 만들어 보는 프로젝트를 수행하였습니다.&nbsp; 최종적으로 server에서 ID와 Passward 및 추가적인 부가 옵션들이 구현 가능한 ftp 서버를 구현하였습니다.</div><div><br></div></div>
