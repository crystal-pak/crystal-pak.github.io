---
# the default layout is 'page'
icon: fas fa-info-circle
order: 4
---


## 📊 장르별 독서 통계

<canvas id="genreChart"></canvas>

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script>
  document.addEventListener('DOMContentLoaded', () => {
    const ctx = document.getElementById('genreChart').getContext('2d');
    const chart = new Chart(ctx, {
      type: 'bar',
      data: {
        labels: [{% for genre in site.posts | map: "genre" | uniq %}"{{ genre }}",{% endfor %}],
        datasets: [{
          label: '장르별 독서량',
          data: [{% for genre in site.posts | map: "genre" | uniq %}{{ site.posts | where: "genre", genre | size }},{% endfor %}],
          backgroundColor: 'rgba(54, 162, 235, 0.5)',
          borderColor: 'rgba(54, 162, 235, 1)',
          borderWidth: 1
        }]
      },
      options: {
        responsive: true,
        scales: {
          y: { beginAtZero: true }
        }
      }
    });
  });
</script>