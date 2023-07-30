# assignment
## Alzheimer's Disease(AD) network analysis
모든 설명은 코드 순서대로 진행됩니다.

뉴런 네트워크 설정과 시뮬레이션에 필요한 Python 모듈을 불러온다. NetpyNe를 불러오면 컴퓨터에 있는 NEURON 프로그램과 자동으로 상호작용하여 시뮬레이션을 설계하고 가동하게 된다.

* cellParams은 cell Parameters(세포 변수)의 약자로, 뉴런의 속성을 결정한다. 신경세포체(soma)라는 섹션(secs)에서 구조적 특징(geom)으로 지름(diam)과 길이(L)를 먼저 결정한다. 뉴런의 이온을 이용한 전기신호 전도 메커니즘(mechs)에서 나트륨 이온의 전도도(gnabar)와 칼륨 이온의 전도도(gkbar)를 설정하고, 이온이 누출되는 정도(gl; leakage) 및 -65mV의 휴지 전위(el)를 결정한다. 이때 이온에 따른 막전위 변화는 호지킨-헉슬리 모델(hh; hodgkin-huxley)을 따랐다.

* popParams는 population parameters(개수 변수)의 약자로, 각 세포의 종류와 개수를 정할 때 사용한다. 흥분성 뉴런(excitatory)의 앞 세 글자 ‘exc’, 억제성 뉴런(inhibitory)의 ‘inh’을 이용하여 두 종류의 세포를 명명하고, 각각 40개, 10개의 개수(numCells)를 지정하였다. 이때 흥분성과 억제성의 비율은 실제의 4:1을 참조하였다. 모든 세포는 피라미드 세포(PYR)로 지정하였다. 

* synMechParams는 synapse mechanism parameters(시냅스 메커니즘 변수)의 약자로, 세부 시냅스 메커니즘을 결정한다. 시냅스의 작동(mod)은 지수함수꼴을 선택하였고(Exp2Syn) 두 시간 상수(tau1, tau2)를 결정하되 tau2에는 평균 1.0, 표준편차 0.5를 따르는 정규분포함수(normalvariate)를 적용하였다. 또한 시냅스를 흥분성(excsyn)과 억제성(inhsyn)으로 나누어 역치(threshold)를 각각 –45mV, -30mV로 설정하였다.

* stimSourceParams는 stimulation source parameters(자극 원천 변수)의 약자로, 배경 자극을 상세 조정한다. 여기서는 일정 시간 간격마다 전기 신호 형태의 자극(NetStim)을 주도록 하여, 노이즈(noise) 설정을 통해 실제 뉴런 네트워크에 가깝도록 하였다. 배경 세포(bkg; background)에서 다른 모든 세포로 자극을 전달하되 전달 확률(probability)과 가중치(weight) 조정을 통해 자극의 세기를 조절할 수 있고, 전달에 걸리는 시간(delay) 또한 설정하였다.

* connParams는 connection parameters(연결 변수)의 약자로, 뉴런 사이의 연결을 모델링한다. 위 코드에서는 시냅스 전 뉴런(preConds)로 개수 변수(pop) 설정에서 정의한 흥분성 뉴런(exc)을, 시냅스 후 뉴런으로도 역시 흥분성 뉴런(exc)를 갖는 시냅스 연결을 생성하고 있다. 이때 가중치(weight)와 확률적 연결(probability)를 통해 연결 수준을 조절하였으며, 흥분성 뉴런이 신호를 시냅스 틈으로 전달하는 것이므로 시냅스 메커니즘(synMech)은 앞서 정의한 흥분성 시냅스(excsyn)을 사용하였다. 이러한 연결 변수를 흥분성과 억제성 뉴런 각각에 모두 정의하여 적절한 형태의 뉴런 네트워크를 생성해낼 수 있다.

시뮬레이션의 가동 시간(simConfig.duration)을 1000ms로 잡고, 측정 단위(simConfig.dt)를 0.025로 설정하였다.

기록할 변수를 정하는 과정(simConfig.recordTraces)으로, 신경세포체(soma) 섹션(sec)의 가운데 위치(loc; 0.5)에서 측정 변수(var)를 전위(v)로 설정하여 신경세포체의 전위(V_soma)를 측정하기로 하였다.

앞서 설명한 세 가지 자료를 분석(simConfig.analysis)하고 추출(saveFig)하는 과정이다. 먼저 흥분성 뉴런에 속하는 0번 뉴런과 억제성 뉴런에 속하는 40번 뉴런을 선택(include)하여 막전위 그래프(plotTraces)를 산출했다. 그다음으로 뉴런 네트워크를 표현하는 이차원 그림(plot2Dnet)을 내려받고, 마지막으로 흥분성 뉴런(exc), 억제성 뉴런(inh)을 포함한 전체 뉴런에서 raster plot(plotRaster)을 그려 결과를 다운로드했다. 이때 뉴런들에 부여된 None은 모든 뉴런을 선택한다는 뜻이다.

지금까지 설정한 모든 네트워크 변수(netParams)와 시뮬레이션 설정(simConfig)을 바탕으로 시뮬레이션을 작동하여 분석(sim.createSimulateAnalyze)한다. 이 과정이 끝나면 설정한 뉴런 네트워크의 기본적인 특성(뉴런 개수, 시냅스 개수 등)과 추출하기로 한 자료가 컴퓨터에 저장된다. 

감사합니다.
