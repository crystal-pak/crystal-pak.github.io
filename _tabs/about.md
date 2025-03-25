---
# the default layout is 'page'
icon: fas fa-info-circle
order: 4
---

## 📊 장르별 독서 통계

{% assign genres = site.posts | map: "genre" | uniq %}

<ul>
  {% assign genre_data = "" %}
  {% for genre in genres %}
    {% unless genre == "" %}
      {% assign genre_count = site.posts | where: "genre", genre | size %}
      <li>{{ genre }}: {{ genre_count }}권</li>
      {% assign genre_data = genre_data | append: genre_count | append: "," %}
    {% endunless %}
  {% endfor %}
</ul>

<!-- Chart.js 차트 삽입 -->
{% include charts.html %}

<div>
  <canvas id="genreChart" width="400" height="400"></canvas>
</div>

<script>
  var ctx = document.getElementById('genreChart').getContext('2d');
  
  // 마크다운에서 전달된 장르별 독서 수 데이터
  var genreCounts = "{{ genre_data | strip_newlines }}".split(",");

  var genreChart = new Chart(ctx, {
    type: 'pie', // 파이차트
    data: {
      labels: {{ genres | jsonify }},
      datasets: [{
        label: '장르별 독서 통계',
        data: genreCounts, // 동적으로 생성된 데이터
        backgroundColor: ['#FF6384', '#36A2EB', '#FFCE56', '#FF5733', '#4CAF50'],
        borderColor: ['#FF6384', '#36A2EB', '#FFCE56', '#FF5733', '#4CAF50'],
        borderWidth: 1
      }]
    },
    options: {
      responsive: true,
      plugins: {
        legend: {
          position: 'top',
        },
        tooltip: {
          enabled: true
        }
      }
    }
  });
</script>