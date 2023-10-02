# 쏜못넬스텐은 취향차이
제 1회 AIKU Conference 1위 수상 프로젝트입니다.<br/>

주변에 한 번쯤씩은 '밴드는 역시 ~~~이다'라는 말을 하는 친구가 입이 닳도록 하는 친구가 있으셨을 겁니다.<br/>
도대체 무슨 노래길래 저러나... 싶은데 또 별로 호감을 안 갈때,<br/>
혹은 내가 듣는 노래랑 비슷은 한가? 혹시 취향이 맞는 곡은 없을까? 싶을때 이용해보면 좋습니다.<br/>
쏜애플, 못, 넬, 국카스텐의 여러 노래 중 내가 고른 노래 하나와 가장 유사한 곡을 찾아줍니다.<br/>
나아가 다른 그룹의 비슷한 노래들도 추천해줍니다.<br/>

## Approach
**'장르로 과연 노래를 구별하는 것이 맞을까?'** 라는 생각에서 시작했습니다.<br/>
평소 다양한 장르의 노래를 평소에 듣기에, 장르를 넘어서는 음악 취향이 있을 것이라 생각했습니다.<br/>
여기서는 **음악적인 특질**을 이용하여 노래를 추천하는 방법을 이용했습니다.<br/>

음악적인 특질은 Spotify의 [echonest](https://en.wikipedia.org/wiki/The_Echo_Nest)를 이용하여 파악했습니다.<br/>
여러 특질 중에서 energy, valence, danceability, acousticness를 이용하였고,<br/>
이는 이 특질만으로 노래를 분류하기에 충분하다는 [해당 논문](https://dl.acm.org/doi/pdf/10.1145/3523227.3546772)을 참고한 것입니다.

[여러 음악](https://www.kaggle.com/datasets/imsparsh/fma-free-music-archive-small-medium)들에 대한 echonest를 이용해 clustering을 진행했습니다.<br/>
이때 clustering 방법으로는 gaussian mixed model을 이용했습니다.<br/>
분석 결과 6개가 가장 원활히 clustering 되어 6개의 군집으로 clustering을 진행한 후 모델을 저장했습니다. (classification_model.pkl)<br/>
그 후 쏜못넬스탠의 노래들의 echonest 정보를 대상으로 모델을 fit하고 해당 결과를 json 형태로 저장했습니다. (song_distributions.json)<br/>

마지막으로 원하는 노래의 echonest 값을 받아와서 동일한 모델에 fit하여 하나의 discrete한 확률 분포를 얻은 후,<br/>
song_distribution.json 내 노래들과의 확률 분포와 가장 유사한 노래를 그룹별로 하나씩 찾습니다. <br/>
유사도는 KL divergence를 이용하여 측정하였습니다.

## How to use
1. git clone 하기
```git bash
$ git clone https://github.com/subin9/TMNS_recommandation
```
2. 필요한 모듈 설치하기
```git bash
$ pip install -r requirments.txt
```
3. spotify client ID, spotify client secret, 노래 URI를 입력하기
```git bash
$ python demo.py --clientid <client ID> --clientsecret <client secret> -uri <Song uri>
```
