# BDAI 수료 프로젝트
#### 주제 
#### ☑️BDAI에서 실제 운영한 행사 데이터를 활용하여 학회 주관 행사의 유입 및 사용자 행동을 분석하고, 이를 바탕으로 신청 전환율 개선안을 제시

- 기간 : 8/10 ~ 8/23
- 개인 프로젝트


📊 데이터정보
- event_info: 행사명, 행사 유형, 진행 방식, 모집 기간 등 행사 기본 정보
- event_application: 사용자별 신청 시점, 신청 상태, 회원 유형 등 행사 신청 데이터
- event_behavior: 행사 관련 페이지에서 발생한 사용자 행동 및 유입 채널·디바이스 등의 행동 데이터

📁 보고서 구성
1. 가설 또는 분석 방향
2. 수행한 분석 과정과 결과
3. 이를 통해 도출한 인사이트 및 개선안

💻Analysis workflow
1. EDA+전처리
2. 탐색적 시각화 & 기술통계
3. 통계적 가설검정
4. 회귀분석
5. 예측 모델링


<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>행사 신청 전환율 리포트</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@400;500;700;900&display=swap" rel="stylesheet">
<style>
  :root{
    --navy:#1B4F72;
    --navy-deep:#123650;
    --orange:#E67E22;
    --orange-soft:#FBE4CF;
    --ink:#20303F;
    --muted:#6B7C8C;
    --line:#E4E9ED;
    --bg-card:#F7F9FB;
    --bg:#FFFFFF;
  }
  *{box-sizing:border-box;}
  html,body{margin:0;padding:0;}
  body{
    background:var(--bg);
    color:var(--ink);
    font-family:'Noto Sans KR', -apple-system, sans-serif;
    line-height:1.5;
    -webkit-font-smoothing:antialiased;
  }
  .wrap{
    max-width:1180px;
    margin:0 auto;
    padding:40px 24px 80px;
  }

  /* ===== Header ===== */
  .eyebrow{
    display:inline-flex;
    align-items:center;
    gap:8px;
    font-size:12px;
    font-weight:700;
    letter-spacing:.14em;
    color:var(--navy);
    background:var(--orange-soft);
    padding:6px 14px;
    border-radius:999px;
    margin-bottom:22px;
  }
  .eyebrow::before{
    content:"";
    width:6px;height:6px;
    border-radius:50%;
    background:var(--orange);
    display:inline-block;
  }

  .hero{
    display:grid;
    grid-template-columns:1.3fr 1fr;
    gap:40px;
    align-items:center;
    padding-bottom:44px;
    border-bottom:1px solid var(--line);
  }
  .hero h1{
    font-size:clamp(28px, 4vw, 42px);
    font-weight:900;
    line-height:1.28;
    margin:0 0 14px;
    letter-spacing:-0.01em;
  }
  .hero h1 .strike{
    color:var(--muted);
    text-decoration:line-through;
    text-decoration-color:#C3CCD3;
    text-decoration-thickness:3px;
    font-weight:700;
  }
  .hero h1 .arrow{color:var(--muted); font-weight:500;}
  .hero h1 .win{color:var(--orange);}
  .hero p{
    font-size:15.5px;
    color:var(--muted);
    max-width:46ch;
    margin:0;
  }

  .hero-visual{
    background:var(--navy);
    border-radius:20px;
    padding:28px 26px;
    color:#fff;
    position:relative;
    overflow:hidden;
  }
  .hero-visual::after{
    content:"";
    position:absolute;
    right:-40px; top:-40px;
    width:160px;height:160px;
    background:radial-gradient(circle, rgba(230,126,34,.35), transparent 70%);
  }
  .hero-visual .row{
    display:flex;
    justify-content:space-between;
    align-items:baseline;
    padding:10px 0;
    border-bottom:1px solid rgba(255,255,255,.14);
  }
  .hero-visual .row:last-child{border-bottom:none;}
  .hero-visual .label{font-size:13px; color:rgba(255,255,255,.7);}
  .hero-visual .value{font-size:22px; font-weight:900;}
  .hero-visual .value.up{color:var(--orange);}

  /* ===== KPI ===== */
  .kpi-title{
    margin:56px 0 20px;
    font-size:13px;
    font-weight:700;
    letter-spacing:.1em;
    color:var(--muted);
    text-transform:uppercase;
  }
  .kpi-grid{
    display:grid;
    grid-template-columns:repeat(4, 1fr);
    gap:16px;
  }
  .kpi-card{
    background:var(--bg-card);
    border:1px solid var(--line);
    border-radius:16px;
    padding:22px 20px;
    position:relative;
  }
  .kpi-card.highlight{
    background:var(--navy);
    border-color:var(--navy);
  }
  .kpi-num{
    font-size:clamp(30px, 3.4vw, 40px);
    font-weight:900;
    color:var(--navy);
    letter-spacing:-0.02em;
  }
  .kpi-card.highlight .kpi-num{color:var(--orange);}
  .kpi-label{
    font-size:14px;
    font-weight:700;
    margin-top:6px;
  }
  .kpi-card.highlight .kpi-label{color:#fff;}
  .kpi-sub{
    font-size:12.5px;
    color:var(--muted);
    margin-top:4px;
  }
  .kpi-card.highlight .kpi-sub{color:rgba(255,255,255,.75);}
  .kpi-tag{
    position:absolute;
    top:16px; right:16px;
    font-size:10.5px;
    font-weight:700;
    padding:3px 9px;
    border-radius:999px;
    background:var(--orange);
    color:#fff;
  }

  /* ===== Charts ===== */
  .chart-section-title{
    margin:56px 0 20px;
    font-size:13px;
    font-weight:700;
    letter-spacing:.1em;
    color:var(--muted);
    text-transform:uppercase;
  }
  .chart-grid{
    display:grid;
    grid-template-columns:repeat(3, 1fr);
    gap:18px;
  }
  .chart-card{
    background:var(--bg);
    border:1px solid var(--line);
    border-radius:16px;
    padding:22px 20px 16px;
  }
  .chart-card .num-badge{
    display:inline-block;
    font-size:11px;
    font-weight:700;
    color:var(--navy);
    background:var(--orange-soft);
    padding:2px 9px;
    border-radius:6px;
    margin-bottom:10px;
  }
  .chart-card h3{
    font-size:17px;
    font-weight:900;
    margin:0 0 6px;
    line-height:1.35;
    color:var(--navy-deep);
  }
  .chart-card p{
    font-size:13px;
    color:var(--muted);
    margin:0 0 14px;
  }
  .chart-holder{
    height:220px;
    position:relative;
  }
  .bar-chart{
    display:flex;
    align-items:flex-end;
    justify-content:space-around;
    height:170px;
    padding:0 6px;
    border-bottom:2px solid var(--line);
  }
  .bar-col{
    display:flex;
    flex-direction:column;
    align-items:center;
    justify-content:flex-end;
    height:100%;
    width:30%;
  }
  .bar-value{
    font-size:15px;
    font-weight:900;
    color:var(--navy-deep);
    margin-bottom:6px;
    white-space:nowrap;
  }
  .bar-shape{
    width:56%;
    max-width:64px;
    border-radius:8px 8px 0 0;
    transition:height 1s cubic-bezier(.2,.8,.2,1);
    height:0;
  }
  .bar-labels{
    display:flex;
    justify-content:space-around;
    margin-top:10px;
    padding:0 6px;
  }
  .bar-labels span{
    width:30%;
    text-align:center;
    font-size:12.5px;
    font-weight:700;
    color:var(--ink);
  }

  /* ===== Action box ===== */
  .action-box{
    margin-top:60px;
    background:var(--navy);
    border-radius:20px;
    padding:32px 34px;
    color:#fff;
    position:relative;
    overflow:hidden;
  }
  .action-box::before{
    content:"";
    position:absolute;
    left:0; top:0; bottom:0;
    width:6px;
    background:var(--orange);
  }
  .action-box h2{
    font-size:21px;
    font-weight:900;
    margin:0 0 4px;
  }
  .action-box .action-sub{
    font-size:13.5px;
    color:rgba(255,255,255,.68);
    margin:0 0 22px;
  }
  .action-list{
    list-style:none;
    margin:0; padding:0;
    display:grid;
    grid-template-columns:repeat(2, 1fr);
    gap:14px 22px;
  }
  .action-list li{
    display:flex;
    gap:12px;
    align-items:flex-start;
    font-size:14px;
    line-height:1.5;
  }
  .action-list .num{
    flex:0 0 auto;
    width:24px; height:24px;
    border-radius:7px;
    background:var(--orange);
    color:#fff;
    font-size:12px;
    font-weight:900;
    display:flex;
    align-items:center;
    justify-content:center;
    margin-top:1px;
  }
  .action-list .txt b{
    display:block;
    font-weight:700;
    margin-bottom:2px;
  }
  .action-list .txt span{
    color:rgba(255,255,255,.7);
    font-size:12.5px;
  }

  .footer-note{
    margin-top:34px;
    font-size:12px;
    color:var(--muted);
    text-align:center;
  }

  /* ===== Responsive ===== */
  @media (max-width:900px){
    .hero{grid-template-columns:1fr;}
    .kpi-grid{grid-template-columns:repeat(2,1fr);}
    .chart-grid{grid-template-columns:1fr;}
    .action-list{grid-template-columns:1fr;}
  }
  @media (max-width:480px){
    .wrap{padding:28px 16px 60px;}
    .kpi-grid{grid-template-columns:1fr 1fr;}
    .kpi-num{font-size:26px;}
  }
</style>
</head>
<body>
<div class="wrap">

  <span class="eyebrow">행사 신청 전환율 리포트</span>

  <div class="hero">
    <div>
      <h1>예상은 <span class="strike">데스크톱</span><span class="arrow"> → </span><span class="win">모바일이 앞섰습니다</span></h1>
      <p>"신청서는 화면이 큰 데스크톱에서 더 잘 쓸 것"이라는 예상과 달리, 실제로는 모바일 이용자의 신청 완료율이 훨씬 높았습니다. 개선의 우선순위는 데스크톱 화면에 있습니다.</p>
    </div>
    <div class="hero-visual">
      <div class="row"><span class="label">분석 대상 행사</span><span class="value">3건</span></div>
      <div class="row"><span class="label">분석한 방문 기록</span><span class="value">330건</span></div>
      <div class="row"><span class="label">모바일 전환율</span><span class="value up">85.8%</span></div>
      <div class="row"><span class="label">데스크톱 전환율</span><span class="value">50.0%</span></div>
    </div>
  </div>

  <div class="kpi-title">한눈에 보는 핵심 지표</div>
  <div class="kpi-grid">
    <div class="kpi-card">
      <div class="kpi-num">65.5%</div>
      <div class="kpi-label">전체 신청 전환율</div>
      <div class="kpi-sub">방문자 330명 중 216명 신청</div>
    </div>
    <div class="kpi-card highlight">
      <span class="kpi-tag">1위</span>
      <div class="kpi-num">85.8%</div>
      <div class="kpi-label">모바일 이용자 전환율</div>
      <div class="kpi-sub">예상을 뒤집은 결과</div>
    </div>
    <div class="kpi-card">
      <div class="kpi-num">50.0%</div>
      <div class="kpi-label">데스크톱 이용자 전환율</div>
      <div class="kpi-sub">가장 개선이 시급한 지점</div>
    </div>
    <div class="kpi-card">
      <div class="kpi-num">100%</div>
      <div class="kpi-label">신청서 작성 완료율</div>
      <div class="kpi-sub">일단 쓰기 시작하면 끝까지 완료</div>
    </div>
  </div>

  <div class="chart-section-title">무엇이 전환율을 갈랐나</div>
  <div class="chart-grid">

    <div class="chart-card">
      <span class="num-badge">01</span>
      <h3>데스크톱 접속자, 전환율 개선이 필요합니다</h3>
      <p>같은 방문인데도 기기에 따라 신청까지 이어지는 비율이 크게 다릅니다.</p>
      <div class="chart-holder">
        <div class="bar-chart" id="chartDevice">
          <div class="bar-col"><div class="bar-value">50.0%</div><div class="bar-shape" data-h="50" style="background:var(--navy)"></div></div>
          <div class="bar-col"><div class="bar-value">60.0%</div><div class="bar-shape" data-h="60" style="background:#D7DEE4"></div></div>
          <div class="bar-col"><div class="bar-value">85.8%</div><div class="bar-shape" data-h="85.8" style="background:var(--orange)"></div></div>
        </div>
        <div class="bar-labels"><span>데스크톱</span><span>태블릿</span><span>모바일</span></div>
      </div>
    </div>

    <div class="chart-card">
      <span class="num-badge">02</span>
      <h3>이탈은 신청서를 쓰기 전에 일어납니다</h3>
      <p>일단 신청서 작성을 시작한 사람은 거의 끝까지 제출합니다. 문제는 그 전 단계입니다.</p>
      <div class="chart-holder">
        <div class="bar-chart" id="chartForm">
          <div class="bar-col" style="width:45%"><div class="bar-value">38.8%</div><div class="bar-shape" data-h="38.8" style="background:var(--navy)"></div></div>
          <div class="bar-col" style="width:45%"><div class="bar-value">63.0%</div><div class="bar-shape" data-h="63" style="background:var(--orange)"></div></div>
        </div>
        <div class="bar-labels"><span style="width:45%">데스크톱</span><span style="width:45%">모바일</span></div>
      </div>
    </div>

    <div class="chart-card">
      <span class="num-badge">03</span>
      <h3>인스타그램(SNS) 유입이 가장 효과적입니다</h3>
      <p>검색으로 들어온 방문자보다 SNS로 들어온 방문자의 신청 전환율이 더 높습니다.</p>
      <div class="chart-holder">
        <div class="bar-chart" id="chartChannel">
          <div class="bar-col"><div class="bar-value">54.1%</div><div class="bar-shape" data-h="54.1" style="background:#D7DEE4"></div></div>
          <div class="bar-col"><div class="bar-value">65.3%</div><div class="bar-shape" data-h="65.3" style="background:var(--navy)"></div></div>
          <div class="bar-col"><div class="bar-value">82.1%</div><div class="bar-shape" data-h="82.1" style="background:var(--orange)"></div></div>
        </div>
        <div class="bar-labels"><span>검색</span><span>직접방문</span><span>SNS</span></div>
      </div>
    </div>

  </div>

  <div class="action-box">
    <h2>지금 바로 할 수 있는 행동</h2>
    <p class="action-sub">데이터가 가리키는 우선순위 4가지</p>
    <ul class="action-list">
      <li>
        <span class="num">1</span>
        <span class="txt">
          <b>데스크톱 화면의 '신청하기' 버튼을 키우고 위로 올리기</b>
          <span>같은 방문자라도 데스크톱에서 전환이 절반 가까이 낮습니다.</span>
        </span>
      </li>
      <li>
        <span class="num">2</span>
        <span class="txt">
          <b>데스크톱 페이지 로딩 속도·첫 화면 구성 점검하기</b>
          <span>신청서 작성 전 단계에서 대부분의 이탈이 발생합니다.</span>
        </span>
      </li>
      <li>
        <span class="num">3</span>
        <span class="txt">
          <b>인스타그램 등 SNS 채널에 마케팅 예산 우선 배정</b>
          <span>같은 방문자 수 대비 신청까지 이어지는 비율이 가장 높습니다.</span>
        </span>
      </li>
      <li>
        <span class="num">4</span>
        <span class="txt">
          <b>검색 유입 첫 화면 문구를 검색어와 맞추기</b>
          <span>검색으로 들어온 방문자의 전환율이 상대적으로 낮습니다.</span>
        </span>
      </li>
    </ul>
  </div>

  <p class="footer-note">본 리포트는 행사 3건, 방문 기록 330건을 기준으로 작성되었습니다.</p>

</div>

<script>
  // 외부 차트 라이브러리 없이, 막대 높이를 %값에 비례하여 채우는 방식
  // 미리보기 환경이 외부 CDN 스크립트를 차단하더라도 항상 동작하도록 순수 JS로 구현
  function animateBars(){
    document.querySelectorAll('.bar-shape').forEach(function(bar, i){
      const pct = parseFloat(bar.getAttribute('data-h'));   // 0~100 사이 값
      const maxTrackHeight = 150;                           // px, .bar-chart 높이(170) 대비 여백 확보
      const targetHeight = (pct / 100) * maxTrackHeight;
      setTimeout(function(){
        bar.style.height = targetHeight + 'px';
      }, 80 * i);
    });
  }
  window.addEventListener('DOMContentLoaded', animateBars);
  window.addEventListener('resize', animateBars);
</script>
</body>
</html>
