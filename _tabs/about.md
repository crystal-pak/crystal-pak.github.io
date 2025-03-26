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

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<canvas id="genreChart" width="400" height="400"></canvas>

<script>
  const genres = [
    {% for genre in genres %}
      "{{ genre }}",
    {% endfor %}
  ];

  const genreCounts = [
    {% for genre in genres %}
      {{ posts_with_genre | where: "genre", genre | size }},
    {% endfor %}
  ];

  /* 차트를 생성합니다. */
  const ctx = document.getElementById('genreChart').getContext('2d');
  const genreChart = new Chart(ctx, {
    type: 'bar', /* 차트 타입 (bar, pie, line 등) */
    data: {
      labels: genres,
      datasets: [{
        label: '장르별 권수',
        data: genreCounts,
        backgroundColor: [
          'rgba(255, 99, 132, 0.2)',
          'rgba(54, 162, 235, 0.2)',
          'rgba(255, 206, 86, 0.2)',
          'rgba(75, 192, 192, 0.2)',
          'rgba(153, 102, 255, 0.2)',
          'rgba(255, 159, 64, 0.2)'
        ],
        borderColor: [
          'rgba(255, 99, 132, 1)',
          'rgba(54, 162, 235, 1)',
          'rgba(255, 206, 86, 1)',
          'rgba(75, 192, 192, 1)',
          'rgba(153, 102, 255, 1)',
          'rgba(255, 159, 64, 1)'
        ],
        borderWidth: 1
      }]
    },
    options: {
      scales: {
        y: {
          beginAtZero: true
        }
      }
    }
  });
</script>