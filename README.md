<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>小学语文课堂语料库教研分析平台</title>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        :root {
            --primary-color: #3498db;
            --secondary-color: #2ecc71;
            --accent-color: #e74c3c;
            --light-color: #f9f9f9;
            --dark-color: #34495e;
            --shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Microsoft YaHei', Arial, sans-serif;
        }
        
        body {
            background-color: #f5f7fa;
            color: #333;
            line-height: 1.6;
        }
        
        header {
            background: linear-gradient(135deg, var(--primary-color), var(--secondary-color));
            color: white;
            padding: 2rem 0;
            text-align: center;
            box-shadow: var(--shadow);
            position: relative;
            overflow: hidden;
        }
        
        header::before {
            content: "";
            position: absolute;
            top: -50%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: radial-gradient(circle, rgba(255,255,255,0.1) 0%, rgba(255,255,255,0) 70%);
            z-index: 0;
        }
        
        header h1 {
            font-size: 2.5rem;
            margin-bottom: 0.5rem;
            position: relative;
            z-index: 1;
        }
        
        header p {
            font-size: 1.2rem;
            max-width: 800px;
            margin: 0 auto;
            position: relative;
            z-index: 1;
        }
        
        .container {
            max-width: 1200px;
            margin: 2rem auto;
            padding: 0 1rem;
        }
        
        .card {
            background: white;
            border-radius: 10px;
            box-shadow: var(--shadow);
            padding: 1.5rem;
            margin-bottom: 2rem;
            transition: transform 0.3s ease;
        }
        
        .card:hover {
            transform: translateY(-5px);
        }
        
        .card h2 {
            color: var(--primary-color);
            margin-bottom: 1rem;
            padding-bottom: 0.5rem;
            border-bottom: 2px solid #eee;
            display: flex;
            align-items: center;
        }
        
        .card h2 i {
            margin-right: 10px;
            font-size: 1.5rem;
        }
        
        .controls {
            display: flex;
            flex-wrap: wrap;
            gap: 1rem;
            margin-bottom: 1.5rem;
        }
        
        select, button, input {
            padding: 0.7rem 1rem;
            border: 1px solid #ddd;
            border-radius: 5px;
            font-size: 1rem;
            background-color: white;
            cursor: pointer;
        }
        
        button {
            background-color: var(--primary-color);
            color: white;
            border: none;
            transition: background-color 0.3s;
            display: flex;
            align-items: center;
            gap: 5px;
        }
        
        button:hover {
            background-color: #2980b9;
        }
        
        .chart-container {
            display: flex;
            flex-wrap: wrap;
            gap: 2rem;
            justify-content: center;
            margin-top: 2rem;
        }
        
        .chart-box {
            flex: 1;
            min-width: 300px;
            background: white;
            border-radius: 8px;
            padding: 1rem;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.05);
        }
        
        .chart-box h3 {
            text-align: center;
            margin-bottom: 1rem;
            color: var(--dark-color);
        }
        
        .insights {
            background-color: #e8f4fc;
            padding: 1.5rem;
            border-radius: 8px;
            margin-top: 1.5rem;
            border-left: 4px solid var(--primary-color);
        }
        
        .insights h3 {
            color: var(--primary-color);
            margin-bottom: 1rem;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .query-section {
            display: flex;
            gap: 1rem;
            margin-bottom: 1.5rem;
        }
        
        .query-section input {
            flex: 1;
            padding: 0.7rem 1rem;
            border: 1px solid #ddd;
            border-radius: 5px;
            font-size: 1rem;
        }
        
        .results {
            background-color: #f9f9f9;
            padding: 1rem;
            border-radius: 5px;
            max-height: 300px;
            overflow-y: auto;
        }
        
        .result-item {
            padding: 1rem;
            border-bottom: 1px solid #eee;
            background: white;
            border-radius: 5px;
            margin-bottom: 10px;
            box-shadow: 0 1px 3px rgba(0,0,0,0.05);
        }
        
        .result-item:last-child {
            border-bottom: none;
            margin-bottom: 0;
        }
        
        .result-item h4 {
            color: var(--primary-color);
            margin-bottom: 5px;
        }
        
        .upload-section {
            display: flex;
            flex-direction: column;
            gap: 1rem;
            margin-top: 1rem;
        }
        
        .upload-area {
            border: 2px dashed #ddd;
            border-radius: 8px;
            padding: 2rem;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s;
        }
        
        .upload-area:hover {
            border-color: var(--primary-color);
            background-color: #f0f8ff;
        }
        
        .upload-area i {
            font-size: 2rem;
            color: var(--primary-color);
            margin-bottom: 10px;
        }
        
        .upload-area p {
            color: #666;
        }
        
        footer {
            text-align: center;
            padding: 2rem;
            background-color: var(--dark-color);
            color: white;
            margin-top: 3rem;
        }
        
        .lesson-example {
            background-color: #f0f8ff;
            padding: 1rem;
            border-radius: 8px;
            margin-top: 1rem;
            border-left: 4px solid var(--secondary-color);
        }
        
        .lesson-example h4 {
            color: var(--secondary-color);
            margin-bottom: 10px;
        }
        
        @media (max-width: 768px) {
            .chart-container {
                flex-direction: column;
            }
            
            .chart-box {
                min-width: 100%;
            }
            
            .query-section {
                flex-direction: column;
            }
        }
        
        /* 图标样式 */
        .icon {
            display: inline-block;
            width: 24px;
            height: 24px;
            background-size: contain;
            background-repeat: no-repeat;
            vertical-align: middle;
            margin-right: 8px;
        }
        
        .icon-analysis { background-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="%233498db"><path d="M19 3H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zM9 17H7v-7h2v7zm4 0h-2V7h2v10zm4 0h-2v-4h2v4z"/></svg>'); }
        .icon-compare { background-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="%233498db"><path d="M10 3H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h5v2h2V1h-2v2zm0 15H5l5-6v6zm9-15h-5v2h5v13l-5-6v9h5c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2z"/></svg>'); }
        .icon-search { background-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="%233498db"><path d="M15.5 14h-.79l-.28-.27C15.41 12.59 16 11.11 16 9.5 16 5.91 13.09 3 9.5 3S3 5.91 3 9.5 5.91 16 9.5 16c1.61 0 3.09-.59 4.23-1.57l.27.28v.79l5 4.99L20.49 19l-4.99-5zm-6 0C7.01 14 5 11.99 5 9.5S7.01 5 9.5 5 14 7.01 14 9.5 11.99 14 9.5 14z"/></svg>'); }
        .icon-upload { background-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="%233498db"><path d="M19.35 10.04C18.67 6.59 15.64 4 12 4 9.11 4 6.6 5.64 5.35 8.04 2.34 8.36 0 10.91 0 14c0 3.31 2.69 6 6 6h13c2.76 0 5-2.24 5-5 0-2.64-2.05-4.78-4.65-4.96zM14 13v4h-4v-4H7l5-5 5 5h-3z"/></svg>'); }
    </style>
</head>
<body>
    <header>
        <h1>小学语文课堂语料库教研分析平台</h1>
        <p>基于多维度标注体系，赋能课堂教学研究与改进</p>
    </header>
    
    <div class="container">
        <!-- 视频上传模块 -->
        <section class="card">
            <h2><span class="icon icon-upload"></span>视频上传与分析</h2>
            <div class="upload-section">
                <div class="upload-area" id="upload-area">
                    <span class="icon icon-upload"></span>
                    <h3>上传课堂视频</h3>
                    <p>点击或拖拽视频文件到此处上传</p>
                    <p class="small">支持格式：MP4, AVI, MOV (最大500MB)</p>
                </div>
                <input type="file" id="video-upload" accept="video/*" style="display: none;">
                <div class="lesson-example">
                    <h4>《少年中国说》课例分析</h4>
                    <p>本课例展示了如何通过语料库分析工具深度解析课堂对话结构、认知层次和情感氛围。</p>
                    <p>根据标注数据，本节课认知活动分布均匀，高阶思维占比显著，情感氛围自始至终都非常积极且充满激情。</p>
                </div>
            </div>
        </section>
        
        <!-- 功能1：单堂课深度分析 -->
        <section class="card">
            <h2><span class="icon icon-analysis"></span>单堂课深度分析报告</h2>
            <div class="controls">
                <select id="lesson-select">
                    <option value="">请选择一堂课</option>
                    <option value="少年中国说" selected>《少年中国说》</option>
                    <option value="示儿">《示儿》</option>
                    <option value="红楼梦">《红楼梦》</option>
                    <option value="稚子弄冰">《稚子弄冰》</option>
                    <option value="题临安邸">《题临安邸》</option>
                    <option value="自相矛盾">《自相矛盾》</option>
                    <option value="草船借箭">《草船借箭》</option>
                    <option value="杨氏之子">《杨氏之子》</option>
                    <option value="童年的发现">《童年的发现》</option>
                </select>
                <button id="analyze-btn"><span class="icon icon-analysis"></span>生成分析报告</button>
            </div>
            
            <div id="single-analysis" style="display: none;">
                <div class="chart-container">
                    <div class="chart-box">
                        <h3>师生对话比例</h3>
                        <canvas id="dialogue-chart"></canvas>
                    </div>
                    <div class="chart-box">
                        <h3>教师提问认知层次分布</h3>
                        <canvas id="cognition-chart"></canvas>
                    </div>
                    <div class="chart-box">
                        <h3>课堂情感氛围变化</h3>
                        <canvas id="emotion-chart"></canvas>
                    </div>
                </div>
                
                <div class="insights">
                    <h3><span class="icon icon-analysis"></span>核心洞察</h3>
                    <p id="insight-text">本堂课高阶思维提问（分析、评价、创造）占比<span id="high-cognition-percent">45%</span>，<span id="comparison-text">高于</span>平均水平。</p>
                    <p>教师通过多种形式的朗读和创造性写作活动，有效培养了学生的分析能力和情感表达能力。</p>
                </div>
            </div>
        </section>
        
        <!-- 功能2：多堂课对比分析 -->
        <section class="card">
            <h2><span class="icon icon-compare"></span>多堂课对比分析</h2>
            <div class="controls">
                <select id="lesson1-select">
                    <option value="">请选择第一堂课</option>
                    <option value="少年中国说" selected>《少年中国说》</option>
                    <option value="示儿">《示儿》</option>
                    <option value="题临安邸">《题临安邸》</option>
                    <option value="自相矛盾">《自相矛盾》</option>
                    <option value="草船借箭">《草船借箭》</option>
                    <option value="杨氏之子">《杨氏之子》</option>
                </select>
                <select id="lesson2-select">
                    <option value="">请选择第二堂课</option>
                    <option value="示儿">《示儿》</option>
                    <option value="题临安邸" selected>《题临安邸》</option>
                    <option value="自相矛盾">《自相矛盾》</option>
                    <option value="草船借箭">《草船借箭》</option>
                    <option value="杨氏之子">《杨氏之子》</option>
                </select>
                <button id="compare-btn"><span class="icon icon-compare"></span>生成对比分析</button>
            </div>
            
            <div id="comparison-analysis" style="display: none;">
                <div class="chart-container">
                    <div class="chart-box">
                        <h3>认知层次分布对比</h3>
                        <canvas id="comparison-cognition-chart"></canvas>
                    </div>
                    <div class="chart-box">
                        <h3>师生互动模式对比</h3>
                        <canvas id="comparison-interaction-chart"></canvas>
                    </div>
                </div>
            </div>
        </section>
        
        <!-- 功能3：智能查询 -->
        <section class="card">
            <h2><span class="icon icon-search"></span>智能查询</h2>
            <div class="query-section">
                <input type="text" id="query-input" placeholder="输入查询条件，例如：展示所有教师正面反馈后，学生创造性回答的案例">
                <button id="query-btn"><span class="icon icon-search"></span>执行查询</button>
            </div>
            
            <div id="query-results" class="results" style="display: none;">
                <h3>查询结果</h3>
                <div id="result-list">
                    <!-- 查询结果将在这里显示 -->
                </div>
            </div>
        </section>
    </div>
    
    <footer>
        <p>小学语文课堂语料库教研分析平台 &copy; 2023</p>
        <p>基于多维度标注体系的课堂对话分析系统</p>
    </footer>

    <script>
        // 模拟数据 - 在实际应用中，这些数据应该从后端API获取
        const lessonData = {
            "少年中国说": {
                teacherRatio: 65,
                studentRatio: 35,
                cognitionLevels: {
                    "记忆": 18,
                    "理解": 22,
                    "应用": 28,
                    "分析": 24,
                    "评价": 8,
                    "创造": 0
                },
                emotionTrend: [0, 0, 1, 1, 1, 1, 0, 1, 1, 0, 0, 1, 0, 0, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1],
                highCognitionPercent: 45
            },
            "示儿": {
                teacherRatio: 70,
                studentRatio: 30,
                cognitionLevels: {
                    "记忆": 15,
                    "理解": 30,
                    "应用": 20,
                    "分析": 20,
                    "评价": 10,
                    "创造": 5
                },
                emotionTrend: [1, 1, 0, 0, 0, 1, 1, 0, 0, 1, 1, 0, 0, 0, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1],
                highCognitionPercent: 35
            },
            "题临安邸": {
                teacherRatio: 70,
                studentRatio: 30,
                cognitionLevels: {
                    "记忆": 20,
                    "理解": 25,
                    "应用": 15,
                    "分析": 25,
                    "评价": 10,
                    "创造": 5
                },
                emotionTrend: [1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 0, 0, -1, 0, 0, -1, 0, -1, -1, -1, 0, 0, -1, 1, 0, 0, -1, -1, 1, 1, 1],
                highCognitionPercent: 40
            },
            "杨氏之子": {
                teacherRatio: 55,
                studentRatio: 45,
                cognitionLevels: {
                    "记忆": 15,
                    "理解": 20,
                    "应用": 25,
                    "分析": 20,
                    "评价": 15,
                    "创造": 5
                },
                emotionTrend: [0, 0, 1, 0, 1, 1, 1, 0, 1, 1, 0, 0, 0, 1, 1, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 0, 0, 0, 1, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 1, 0, 0, 0, 1, 1, 0, 0, 1, 1, 0, 1, 1, 1, 0, 0, 1, 1, 0, 0, 0, 1, 1, 1, 1],
                highCognitionPercent: 40
            }
        };

        // 视频上传功能
        document.getElementById('upload-area').addEventListener('click', function() {
            document.getElementById('video-upload').click();
        });
        
        document.getElementById('video-upload').addEventListener('change', function(e) {
            if (e.target.files.length > 0) {
                const fileName = e.target.files[0].name;
                alert(`已上传视频: ${fileName}\n视频分析功能正在开发中...`);
                // 在实际应用中，这里应该调用后端API进行视频分析
            }
        });

        // 单堂课分析功能
        document.getElementById('analyze-btn').addEventListener('click', function() {
            const selectedLesson = document.getElementById('lesson-select').value;
            if (!selectedLesson) {
                alert('请选择一堂课');
                return;
            }
            
            const data = lessonData[selectedLesson];
            if (!data) {
                alert('暂无该课程数据');
                return;
            }
            
            // 显示分析区域
            document.getElementById('single-analysis').style.display = 'block';
            
            // 绘制师生对话比例饼图
            const dialogueCtx = document.getElementById('dialogue-chart').getContext('2d');
            if (window.dialogueChart) {
                window.dialogueChart.destroy();
            }
            window.dialogueChart = new Chart(dialogueCtx, {
                type: 'pie',
                data: {
                    labels: ['教师', '学生'],
                    datasets: [{
                        data: [data.teacherRatio, data.studentRatio],
                        backgroundColor: ['#3498db', '#2ecc71']
                    }]
                },
                options: {
                    responsive: true,
                    plugins: {
                        legend: {
                            position: 'bottom'
                        },
                        title: {
                            display: true,
                            text: '师生对话比例'
                        }
                    }
                }
            });
            
            // 绘制教师提问认知层次分布条形图
            const cognitionCtx = document.getElementById('cognition-chart').getContext('2d');
            if (window.cognitionChart) {
                window.cognitionChart.destroy();
            }
            window.cognitionChart = new Chart(cognitionCtx, {
                type: 'bar',
                data: {
                    labels: Object.keys(data.cognitionLevels),
                    datasets: [{
                        label: '问题数量',
                        data: Object.values(data.cognitionLevels),
                        backgroundColor: '#3498db'
                    }]
                },
                options: {
                    responsive: true,
                    scales: {
                        y: {
                            beginAtZero: true
                        }
                    },
                    plugins: {
                        title: {
                            display: true,
                            text: '教师提问认知层次分布'
                        }
                    }
                }
            });
            
            // 绘制课堂情感氛围变化曲线
            const emotionCtx = document.getElementById('emotion-chart').getContext('2d');
            if (window.emotionChart) {
                window.emotionChart.destroy();
            }
            window.emotionChart = new Chart(emotionCtx, {
                type: 'line',
                data: {
                    labels: data.emotionTrend.map((_, i) => `话轮${i+1}`),
                    datasets: [{
                        label: '情感值',
                        data: data.emotionTrend,
                        borderColor: '#e74c3c',
                        backgroundColor: 'rgba(231, 76, 60, 0.1)',
                        tension: 0.3,
                        fill: true
                    }]
                },
                options: {
                    responsive: true,
                    scales: {
                        y: {
                            min: -1,
                            max: 1,
                            ticks: {
                                callback: function(value) {
                                    if (value === 1) return '积极';
                                    if (value === 0) return '中性';
                                    if (value === -1) return '消极';
                                    return value;
                                }
                            }
                        }
                    },
                    plugins: {
                        title: {
                            display: true,
                            text: '课堂情感氛围变化'
                        }
                    }
                }
            });
            
            // 更新核心洞察
            document.getElementById('high-cognition-percent').textContent = `${data.highCognitionPercent}%`;
            document.getElementById('comparison-text').textContent = 
                data.highCognitionPercent > 35 ? '高于' : '低于';
        });
        
        // 多堂课对比分析功能
        document.getElementById('compare-btn').addEventListener('click', function() {
            const lesson1 = document.getElementById('lesson1-select').value;
            const lesson2 = document.getElementById('lesson2-select').value;
            
            if (!lesson1 || !lesson2) {
                alert('请选择两堂课进行对比');
                return;
            }
            
            const data1 = lessonData[lesson1];
            const data2 = lessonData[lesson2];
            
            if (!data1 || !data2) {
                alert('暂无其中一堂课的数据');
                return;
            }
            
            // 显示对比分析区域
            document.getElementById('comparison-analysis').style.display = 'block';
            
            // 绘制认知层次分布对比图
            const comparisonCognitionCtx = document.getElementById('comparison-cognition-chart').getContext('2d');
            if (window.comparisonCognitionChart) {
                window.comparisonCognitionChart.destroy();
            }
            window.comparisonCognitionChart = new Chart(comparisonCognitionCtx, {
                type: 'bar',
                data: {
                    labels: Object.keys(data1.cognitionLevels),
                    datasets: [
                        {
                            label: lesson1,
                            data: Object.values(data1.cognitionLevels),
                            backgroundColor: 'rgba(52, 152, 219, 0.7)'
                        },
                        {
                            label: lesson2,
                            data: Object.values(data2.cognitionLevels),
                            backgroundColor: 'rgba(46, 204, 113, 0.7)'
                        }
                    ]
                },
                options: {
                    responsive: true,
                    scales: {
                        y: {
                            beginAtZero: true
                        }
                    },
                    plugins: {
                        title: {
                            display: true,
                            text: '认知层次分布对比'
                        }
                    }
                }
            });
            
            // 绘制师生互动模式对比图
            const comparisonInteractionCtx = document.getElementById('comparison-interaction-chart').getContext('2d');
            if (window.comparisonInteractionChart) {
                window.comparisonInteractionChart.destroy();
            }
            window.comparisonInteractionChart = new Chart(comparisonInteractionCtx, {
                type: 'doughnut',
                data: {
                    labels: ['教师', '学生'],
                    datasets: [
                        {
                            label: lesson1,
                            data: [data1.teacherRatio, data1.studentRatio],
                            backgroundColor: ['rgba(52, 152, 219, 0.7)', 'rgba(46, 204, 113, 0.7)']
                        }
                    ]
                },
                options: {
                    responsive: true,
                    plugins: {
                        legend: {
                            position: 'bottom'
                        },
                        title: {
                            display: true,
                            text: `${lesson1}师生互动模式`
                        }
                    }
                }
            });
        });
        
        // 智能查询功能
        document.getElementById('query-btn').addEventListener('click', function() {
            const query = document.getElementById('query-input').value.trim();
            if (!query) {
                alert('请输入查询条件');
                return;
            }
            
            // 显示查询结果区域
            document.getElementById('query-results').style.display = 'block';
            
            // 模拟查询结果
            const results = [
                {
                    lesson: "少年中国说",
                    context: "教师正面反馈：'太棒了！这正是我们说的伏笔！'，学生创造性回答：'我认为诗人想表达的是对自然真实童趣的欣赏。'"
                },
                {
                    lesson: "杨氏之子",
                    context: "教师正面反馈：'这个想法非常深刻！'，学生创造性回答：'如果我是导演，我会用长镜头表现主人公的孤独感。'"
                },
                {
                    lesson: "少年中国说",
                    context: "教师正面反馈：'你的朗读让我仿佛看到了那片美景！'，学生创造性回答：'我认为我们可以通过创作诗歌来表达爱国情怀。'"
                }
            ];
            
            // 清空并填充结果列表
            const resultList = document.getElementById('result-list');
            resultList.innerHTML = '';
            
            results.forEach(result => {
                const resultItem = document.createElement('div');
                resultItem.className = 'result-item';
                resultItem.innerHTML = `
                    <h4>${result.lesson}</h4>
                    <p>${result.context}</p>
                `;
                resultList.appendChild(resultItem);
            });
        });
        
        // 默认显示《少年中国说》的分析
        document.getElementById('analyze-btn').click();
    </script>
</body>
</html>
