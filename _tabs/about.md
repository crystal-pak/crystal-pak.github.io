---
# the default layout is 'page'
icon: fas fa-info-circle
order: 4
---

## 📊 장르별 독서 통계

{% assign genres = site.posts | map: "genre" | uniq %}

<ul>
  {% for genre in genres %}
    {% unless genre == "" %}
      {% assign genre_count = site.posts | where: "genre", genre | size %}
      <li>{{ genre }}: {{ genre_count }}권</li>
    {% endunless %}
  {% endfor %}
</ul>

<canvas id="genreChart" style="max-width: 100%; height: 300px;"></canvas>

<!-- Chart.js가 없을 때만 로드 -->
{% if page.chartjs != false %}
  <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0"></script>
{% endif %}

<script>
  document.addEventListener('DOMContentLoaded', () => {
    const ctx = document.getElementById('genreChart').getContext('2d');
    
    // 장르별 데이터
    const genres = [{% for genre in site.posts | map: "genre" | uniq %}"{{ genre }}",{% endfor %}];
    const counts = [{% for genre in site.posts | map: "genre" | uniq %}{{ site.posts | where: "genre", genre | size }},{% endfor %}];

    if (genres.length === 0) {
      console.error("장르 데이터가 없습니다.");
      return;
    }

    // ModeToggle 중복 선언 방지
    if (typeof window.ModeToggle === "undefined") {
      window.ModeToggle = function() {
        console.log("모드 토글 실행");
      };
    }

    // 차트 생성
    new Chart(ctx, {
      type: 'bar',
      data: {
        labels: genres,
        datasets: [{
          label: '장르별 독서량',
          data: counts,
          backgroundColor: 'rgba(54, 162, 235, 0.5)',
          borderColor: 'rgba(54, 162, 235, 1)',
          borderWidth: 1
        }]
      },
      options: {
        responsive: true,
        maintainAspectRatio: false,  // 크기 자동 유지 설정을 비활성화
        aspectRatio: 2,  // 차트의 비율을 2:1로 설정
        scales: {
          y: { beginAtZero: true }
        }
      }
    });
  });
</script>
