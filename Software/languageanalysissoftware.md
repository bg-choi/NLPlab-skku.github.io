---
layout: post
title: languageanalysissoftware
---
<h4>Software</h4>
 <div class="linklink" style = "background-color:#ffffff;border-radius:0 15px">
          <ul class="posts-list">
           <li>Language Analysis Software(here)
           </li>
           <li class="post-link">
                <a class="post-title" href="https://youngjoongko.github.io/Software/dialoguesystem</">Dialogue System</a>
           </li>
           <li class="post-link">
                <a class="post-title" href="https://youngjoongko.github.io/Software/bigdata/">Big Data</a>
           </li>
           <li class="post-link">
                <a class="post-title" href="https://youngjoongko.github.io/Software/neuralmachinetranslation/">Neural Machine Translation</a>
           </li>
          </ul>
  </div>


<div class="post">
  <h1 class="pageTitle">Language Analysis Software</h1>	
  <p class="meta">언어 분석 소프트웨어</p>
</div> 

### 데모
* DANCHU<br>
  [DANCHU 소개 pdf 다운로드][danchupdf] [DANCHU 동영상][danchumv]
* Word Space Detector<br>
  [Test page][wordspace] [Test page 동영상][wordspacemv]
* Dependency Parser <br>
  [DP_Github][dp]
* Named Entity Recognizer <br>
  [NER_Github][ner]
<br>

### 관련연구
#### DANCHU 
Bidirectional LSTM CRFs를 사용하여 개발한 한국어 POS, NER, DP, SRL 입니다. 데모는 시스템 부하로 인해 올리지 않겠습니다. 관심있으신 분들은 연락해주시길 바랍니다.

#### Word Space Detector 
기존에 대용량 사전을 사용하는 논문들과는 달리 음절이 가지는 확률 정보에 기반한 띄어쓰기 프로그램입니다. 경량화에 초점을 맞추었기 때문에 확률정보만을 기반으로 하였고, 1MB 남짓한 용량으로 94%이상의 성능을 나타내었습니다.

#### Noun Extractor 
문서를 대표하는 색인어인 명사를 추출하기 위하여 연구되었던 많은 방법중에서, 언어 분석도구를 사용하지 않고 한국어 명사의 출현 특성과 적용 규칙을 이용하여 제작한 경량화된 명사 추출기입니다. 146 KB의 사전 용량으로 0.86의 F1-measure 값을 나타내었습니다.

#### Dependency Parser 
의존 구문 분석(Dependency Parsing)은 자연어 문장의 구조적 관계를 파악하는 언어 분석의 한 단계입니다. 문장 안의 모든 어절은 의존소, 지배소가 되며, 의존소와 지배소 쌍은 하나의 의존 관계명으로 정의할 수 있습니다. 의존 관계 및 의존 관계명을 잘 부착하는 것은 문장의 의미적, 구조적 중의성을 해결할 수 있으며, 잘 분석된 의존 구문 분석은 많은 자연어 처리 연구에서 중요한 자질로 사용되고 있습니다.

####  Named Entity Recognizer 
개체명 인식(Named-Entity Recognition)이란 정보 추출의 부과업으로 문서에서 개체명(Named-Entity)을 찾아 추출하고, 인명, 기관명, 지명, 시간 표현, 날짜 표현 등의 미리 정의된 고유한 범주로 분류하는 과정입니다.
<br>

### 참여한 대표 프로젝트
* April 2017 ~ December 2019<br> 
 Co-Principal Investigator (Co-PI) involved in"기계학습용 텍스트 데이터 레이블 자동생성 및 검증도구 개발. (Development of Automatic Text Data Labeling and Verification Tools for Machine Learning)."<br>
 Supported by 정보통신/방송 기술개발사업, 미래창조과학부 (Ministry of Science, ICT and Future Planning).<br>
 (IS lab. Dong-A University)
 * April 2016 ~ December 2016 <br>
  Co-Principal Investigator (Co-PI) involved in"의학정보 질의응답시스템 개발을 위한 의학용어 개체명(ME: Medical Entity) 인식 기술 및 문장 임베딩 기술 개발 (Development of Medical Entity Recognition and Sentence Embedding Techniques for Medical Information Retrieval)."<br>
  Supported by LG Electronics.<br>
  (IS lab. Dong-A University)


[danchumv]: http://dais.donga.ac.kr/files/dais/board/univislab/Danchu(0).zip
[danchupdf]:  http://dais.donga.ac.kr/files/dais/board/univislab/DANCHU-2.pdf
[wordspacemv]:  https://dais.donga.ac.kr/files/dais/board/univislab/Spacing.zip
[wordspace]:  demo_word_space_detector.jsp
[dp]:  https://github.com/sgnlplabeling/nlp_labeling/tree/master/donga/torch_dp_system
[ner]:  https://github.com/sgnlplabeling/nlp_labeling/tree/master/donga/NER


