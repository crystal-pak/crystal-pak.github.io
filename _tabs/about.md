---
# the default layout is 'page'
icon: fas fa-info-circle
order: 4
---

## 📊 장르별 독서 통계

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

    // Liquid로 생성한 데이터를 JavaScript 변수에 전달
    const labels = JSON.parse('{{ genres | jsonify }}');
    const data = JSON.parse('[{% for genre in genres %}{{ posts_with_genre | where: "genre", genre | size }}{% if forloop.last == false %}, {% endif %}{% endfor %}]');

    // 디버깅: 데이터 확인
    console.log("📊 장르 목록 (labels):", labels);
    console.log("📈 장르별 수량 (data):", data);

    // 데이터가 비었는지 확인
    if (labels.length === 0 || data.length === 0) {
      console.error("❌ 차트를 생성할 데이터가 없습니다.");
      return;
    }

    // Chart.js로 차트 생성
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
  });
</script>