---
# the default layout is 'page'
icon: fas fa-info-circle
order: 4
---

## 📊 장르별 독서 통계

{% assign posts_with_genre = site.posts | where_exp: "post", "post.genre != ''" %}
{% assign genres = posts_with_genre | map: "genre" | uniq %}

<ul>
  {% for genre in genres %}
    {% assign genre_count = posts_with_genre | where: "genre", genre | size %}
    <li>{{ genre }}: {{ genre_count }}권</li>
  {% endfor %}
</ul>

## 📊 시각화된 통계

<canvas id="genreChart" width="400" height="400"></canvas>

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script>
  document.addEventListener("DOMContentLoaded", function() {
    const ctx = document.getElementById('genreChart').getContext('2d');

    // 장르와 수량 데이터
    const labels = [
      {% for genre in genres %}
        "{{ genre }}"{% unless forloop.last %}, {% endunless %}
      {% endfor %}
    ];

    const data = [
      {% for genre in genres %}
        {{ posts_with_genre | where: "genre", genre | size }}{% unless forloop.last %}, {% endunless %}
      {% endfor %}
    ];

    new Chart(ctx, {
      type: 'bar', // 'pie', 'doughnut'도 가능
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