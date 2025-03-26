---
# the default layout is 'page'
icon: fas fa-info-circle
order: 4
---

## 📊 장르별 독서 통계

<!-- 장르가 있는 글만 필터링 -->
{% assign posts_with_genre = site.posts | where_exp: "post", "post.genre and post.genre != ''" %}
{% assign genres = posts_with_genre | map: "genre" | uniq %}

<ul>
  {% for genre in genres %}
    {% assign genre_count = posts_with_genre | where: "genre", genre | size %}
    <li>{{ genre }}: {{ genre_count }}권</li>
  {% endfor %}
</ul>

## 📊 시각화된 통계

<canvas id="genreChart" width="400" height="400"></canvas>

<!-- Chart.js 로드 -->
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<script>
  document.addEventListener("DOMContentLoaded", function() {
    const ctx = document.getElementById('genreChart').getContext('2d');

    // 장르와 수량 데이터 생성 (쉼표 문제 해결)
    const labels = [
      {% for genre in genres %}
        "{{ genre }}" {% if forloop.last == false %}, {% endif %}
      {% endfor %}
    ];

    const data = [
      {% for genre in genres %}
        {{ posts_with_genre | where: "genre", genre | size }} {% if forloop.last == false %}, {% endif %}
      {% endfor %}
    ];

    // 디버깅: 콘솔에 데이터 출력
    console.log("📊 장르 목록:", labels);
    console.log("📈 장르별 수량:", data);

    // 데이터 확인 후 차트 생성
    if (labels.length > 0 && data.length > 0) {
      new Chart(ctx, {
        type: 'bar',
        data: {
          labels: labels,
          datasets: [{
            label: '독서 장르별 통계',
            data: data,
            backgroundColor: [
              'rgba(255, 99, 132, 0.6)',
              'rgba(54, 162, 235, 0.6)',
              'rgba(255, 206, 86, 0.6)',
              'rgba(75, 192, 192, 0.6)',
              'rgba(153, 102, 255, 0.6)',
              'rgba(255, 159, 64, 0.6)'
            ],
            borderWidth: 1
          }]
        },
        options: {
          responsive: true,
          scales: {
            y: {
              beginAtZero: true
            }
          }
        }
      });
    } else {
      console.warn("⚠️ 차트를 생성할 데이터가 없습니다.");
    }
  });
</script>