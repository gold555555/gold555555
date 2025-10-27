## Hi there 👋
<!DOCTYPE html>
<html>
<head>
    <title>Most Used Languages in My GitHub Repo</title>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
</head>
<body>
    <canvas id="myChart" width="400" height="200"></canvas>
    <script>
        const ctx = document.getElementById('myChart').getContext('2d');
        const USERNAME = 'YOUR_USERNAME'; // เปลี่ยนเป็น username GitHub ของคุณ
        const REPONAME = 'YOUR_REPO';     // เปลี่ยนเป็นชื่อ repo ของคุณ
        
        let chart; // ตัวแปรสำหรับกราฟ

        // ฟังก์ชันดึงข้อมูลจาก GitHub API และอัปเดตกราฟ
        async function updateChart() {
            try {
                const response = await fetch(`https://api.github.com/repos/${USERNAME}/${REPONAME}/languages`);
                const languagesData = await response.json();
                
                // คำนวณ total bytes
                const totalBytes = Object.values(languagesData).reduce((sum, val) => sum + val, 0);
                
                // เตรียม labels และ data สำหรับกราฟ (เรียงจากมากไปน้อย)
                const sortedLanguages = Object.entries(languagesData)
                    .sort(([,a], [,b]) => b - a)
                    .slice(0, 5); // แสดงแค่ 5 ภาษาที่ใช้มากที่สุด (ปรับได้)
                
                const labels = sortedLanguages.map(([lang]) => lang);
                const data = sortedLanguages.map(([, bytes]) => ((bytes / totalBytes) * 100).toFixed(2));
                
                // สีแบบในรูปของคุณ (ปรับได้)
                const colors = [
                    'rgba(54, 162, 235, 0.6)',   // น้ำเงิน
                    'rgba(255, 206, 86, 0.6)',   // เหลือง
                    'rgba(255, 99, 132, 0.6)',   // แดง
                    'rgba(153, 102, 255, 0.6)',  // ม่วง
                    'rgba(75, 192, 192, 0.6)'    // เขียว
                ];
                
                // สร้างหรืออัปเดตกราฟ (horizontal bar)
                if (chart) {
                    chart.data.labels = labels;
                    chart.data.datasets[0].data = data;
                    chart.data.datasets[0].backgroundColor = colors.slice(0, labels.length);
                    chart.update();
                } else {
                    chart = new Chart(ctx, {
                        type: 'bar',
                        data: {
                            labels: labels,
                            datasets: [{
                                label: 'Percentage (%)',
                                data: data,
                                backgroundColor: colors.slice(0, labels.length),
                                borderWidth: 1
                            }]
                        },
                        options: {
                            indexAxis: 'y', // ทำให้เป็น horizontal bar
                            scales: {
                                x: {
                                    beginAtZero: true,
                                    max: 100
                                }
                            },
                            plugins: {
                                legend: {
                                    display: false // ซ่อน legend ถ้าไม่ต้องการ
                                }
                            }
                        }
                    });
                }
                
                console.log('Updated with real data:', labels, data); // สำหรับ debug
            } catch (error) {
                console.error('Error fetching data:', error);
                // ถ้าดึงไม่ได้ แสดงข้อความ error หรือ mock data ชั่วคราว
            }
        }

        // โหลดข้อมูลครั้งแรก
        updateChart();

        // อัปเดต real-time ทุก 30 วินาที (ปรับ interval ได้)
        setInterval(updateChart, 30000);
    </script>
</body>
</html>
<!--
**gold555555/gold555555** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->
