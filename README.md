<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>식사 포장 주문 현황</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', sans-serif;
            background: #f5f6fa;
            min-height: 100vh;
            padding: 15px;
        }

        .container {
            max-width: 800px;
            margin: 0 auto;
            background: white;
            border-radius: 12px;
            box-shadow: 0 4px 20px rgba(0,0,0,0.08);
            overflow: hidden;
        }

        .header {
            background: #2f3542;
            color: white;
            padding: 25px;
            text-align: center;
        }

        .header h1 {
            font-size: 2.2em;
            margin-bottom: 10px;
            font-weight: 700;
        }

        .header p {
            font-size: 1.1em;
            opacity: 0.9;
        }

        .content {
            padding: 30px;
        }



        .day-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }

        .day-card {
            background: #f8f9fa;
            border-radius: 15px;
            padding: 20px;
            border: 2px solid transparent;
            transition: all 0.3s ease;
        }

        .day-card:hover {
            border-color: #2f3542;
            box-shadow: 0 3px 15px rgba(47, 53, 66, 0.1);
        }

        .day-title {
            font-size: 1.3em;
            font-weight: 700;
            margin-bottom: 15px;
            color: #495057;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .day-emoji {
            font-size: 1.5em;
        }

        .member-list {
            space-y: 10px;
        }

        .member-item {
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 12px;
            background: white;
            border-radius: 10px;
            border: 1px solid #e9ecef;
            margin-bottom: 8px;
            transition: all 0.2s ease;
        }

        .member-item:hover {
            border-color: #2f3542;
            box-shadow: 0 1px 5px rgba(47, 53, 66, 0.1);
        }

        .member-name {
            font-weight: 600;
            color: #495057;
        }

        .status-toggle {
            display: flex;
            gap: 5px;
        }

        .status-btn {
            width: 40px;
            height: 40px;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-size: 1.2em;
            font-weight: bold;
            transition: all 0.2s ease;
        }

        .status-btn.order {
            background: #28a745;
            color: white;
        }

        .status-btn.no-order {
            background: #dc3545;
            color: white;
        }

        .status-btn:not(.active) {
            background: #e9ecef;
            color: #6c757d;
        }

        .status-btn:hover {
            transform: scale(1.1);
        }

        .add-member {
            margin-top: 15px;
            display: flex;
            gap: 10px;
        }

        .add-member input {
            flex: 1;
            padding: 12px;
            border: 1px solid #e9ecef;
            border-radius: 8px;
            font-size: 14px;
        }

        .add-member button {
            padding: 12px 20px;
            background: #2f3542;
            color: white;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-weight: 600;
            transition: all 0.2s ease;
        }

        .add-member button:hover {
            background: #40465a;
            transform: translateY(-1px);
        }

        .summary {
            background: linear-gradient(135deg, #f8f9fa 0%, #e9ecef 100%);
            padding: 20px;
            border-radius: 15px;
            margin-top: 20px;
        }

        .summary h3 {
            color: #495057;
            margin-bottom: 15px;
            font-size: 1.2em;
        }

        .summary-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(100px, 1fr));
            gap: 15px;
        }

        .summary-item {
            text-align: center;
            padding: 15px;
            background: white;
            border-radius: 10px;
            border: 1px solid #e9ecef;
        }

        .summary-day {
            font-weight: 600;
            color: #495057;
            margin-bottom: 5px;
        }

        .summary-count {
            font-size: 1.5em;
            font-weight: 700;
            color: #2f3542;
        }

        @media (max-width: 768px) {
            .day-grid {
                grid-template-columns: 1fr;
            }
            
            .summary-grid {
                grid-template-columns: repeat(2, 1fr);
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>🍱 식사 포장 주문 현황 - by 이광희</h1>
            <p>매일 누가 포장 주문하는지 체크해보세요!</p>
        </div>

        <div class="content">


            <div class="day-grid">
                <div class="day-card">
                    <div class="day-title">
                        <span class="day-emoji">🌅</span>
                        월요일
                    </div>
                    <div class="member-list" data-day="monday">
                        <div class="member-item">
                            <span class="member-name">김철수</span>
                            <div class="status-toggle">
                                <button class="status-btn order" data-status="order">O</button>
                                <button class="status-btn no-order active" data-status="no-order">X</button>
                            </div>
                        </div>
                        <div class="member-item">
                            <span class="member-name">이영희</span>
                            <div class="status-toggle">
                                <button class="status-btn order active" data-status="order">O</button>
                                <button class="status-btn no-order" data-status="no-order">X</button>
                            </div>
                        </div>
                        <div class="member-item">
                            <span class="member-name">박민수</span>
                            <div class="status-toggle">
                                <button class="status-btn order" data-status="order">O</button>
                                <button class="status-btn no-order active" data-status="no-order">X</button>
                            </div>
                        </div>
                    </div>
                    <div class="add-member">
                        <input type="text" placeholder="이름 입력" data-day="monday">
                        <button onclick="addMember('monday')">추가</button>
                    </div>
                </div>

                <div class="day-card">
                    <div class="day-title">
                        <span class="day-emoji">🔥</span>
                        화요일
                    </div>
                    <div class="member-list" data-day="tuesday">
                        <div class="member-item">
                            <span class="member-name">김철수</span>
                            <div class="status-toggle">
                                <button class="status-btn order active" data-status="order">O</button>
                                <button class="status-btn no-order" data-status="no-order">X</button>
                            </div>
                        </div>
                        <div class="member-item">
                            <span class="member-name">이영희</span>
                            <div class="status-toggle">
                                <button class="status-btn order" data-status="order">O</button>
                                <button class="status-btn no-order active" data-status="no-order">X</button>
                            </div>
                        </div>
                    </div>
                    <div class="add-member">
                        <input type="text" placeholder="이름 입력" data-day="tuesday">
                        <button onclick="addMember('tuesday')">추가</button>
                    </div>
                </div>

                <div class="day-card">
                    <div class="day-title">
                        <span class="day-emoji">⚡</span>
                        수요일
                    </div>
                    <div class="member-list" data-day="wednesday">
                        <div class="member-item">
                            <span class="member-name">김철수</span>
                            <div class="status-toggle">
                                <button class="status-btn order" data-status="order">O</button>
                                <button class="status-btn no-order active" data-status="no-order">X</button>
                            </div>
                        </div>
                        <div class="member-item">
                            <span class="member-name">박민수</span>
                            <div class="status-toggle">
                                <button class="status-btn order active" data-status="order">O</button>
                                <button class="status-btn no-order" data-status="no-order">X</button>
                            </div>
                        </div>
                    </div>
                    <div class="add-member">
                        <input type="text" placeholder="이름 입력" data-day="wednesday">
                        <button onclick="addMember('wednesday')">추가</button>
                    </div>
                </div>

                <div class="day-card">
                    <div class="day-title">
                        <span class="day-emoji">🌟</span>
                        목요일
                    </div>
                    <div class="member-list" data-day="thursday">
                        <div class="member-item">
                            <span class="member-name">이영희</span>
                            <div class="status-toggle">
                                <button class="status-btn order active" data-status="order">O</button>
                                <button class="status-btn no-order" data-status="no-order">X</button>
                            </div>
                        </div>
                    </div>
                    <div class="add-member">
                        <input type="text" placeholder="이름 입력" data-day="thursday">
                        <button onclick="addMember('thursday')">추가</button>
                    </div>
                </div>

                <div class="day-card">
                    <div class="day-title">
                        <span class="day-emoji">🎉</span>
                        금요일
                    </div>
                    <div class="member-list" data-day="friday">
                        <div class="member-item">
                            <span class="member-name">김철수</span>
                            <div class="status-toggle">
                                <button class="status-btn order active" data-status="order">O</button>
                                <button class="status-btn no-order" data-status="no-order">X</button>
                            </div>
                        </div>
                        <div class="member-item">
                            <span class="member-name">이영희</span>
                            <div class="status-toggle">
                                <button class="status-btn order active" data-status="order">O</button>
                                <button class="status-btn no-order" data-status="no-order">X</button>
                            </div>
                        </div>
                        <div class="member-item">
                            <span class="member-name">박민수</span>
                            <div class="status-toggle">
                                <button class="status-btn order active" data-status="order">O</button>
                                <button class="status-btn no-order" data-status="no-order">X</button>
                            </div>
                        </div>
                    </div>
                    <div class="add-member">
                        <input type="text" placeholder="이름 입력" data-day="friday">
                        <button onclick="addMember('friday')">추가</button>
                    </div>
                </div>
            </div>

            <div class="summary">
                <h3>📊 주간 주문 현황</h3>
                <div class="summary-grid">
                    <div class="summary-item">
                        <div class="summary-day">월</div>
                        <div class="summary-count" id="monday-count">1</div>
                    </div>
                    <div class="summary-item">
                        <div class="summary-day">화</div>
                        <div class="summary-count" id="tuesday-count">1</div>
                    </div>
                    <div class="summary-item">
                        <div class="summary-day">수</div>
                        <div class="summary-count" id="wednesday-count">1</div>
                    </div>
                    <div class="summary-item">
                        <div class="summary-day">목</div>
                        <div class="summary-count" id="thursday-count">1</div>
                    </div>
                    <div class="summary-item">
                        <div class="summary-day">금</div>
                        <div class="summary-count" id="friday-count">3</div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <script>
        // 상태 토글 기능
        document.addEventListener('click', function(e) {
            if (e.target.classList.contains('status-btn')) {
                const memberItem = e.target.closest('.member-item');
                const buttons = memberItem.querySelectorAll('.status-btn');
                
                // 모든 버튼에서 active 클래스 제거
                buttons.forEach(btn => btn.classList.remove('active'));
                
                // 클릭한 버튼에 active 클래스 추가
                e.target.classList.add('active');
                
                // 카운트 업데이트
                updateCounts();
            }
        });

        // 주간 선택 기능 (제거됨)

        // 멤버 추가 기능
        function addMember(day) {
            const input = document.querySelector(`input[data-day="${day}"]`);
            const memberList = document.querySelector(`div[data-day="${day}"]`);
            const name = input.value.trim();
            
            if (name) {
                const memberItem = document.createElement('div');
                memberItem.className = 'member-item';
                memberItem.innerHTML = `
                    <span class="member-name">${name}</span>
                    <div class="status-toggle">
                        <button class="status-btn order" data-status="order">O</button>
                        <button class="status-btn no-order active" data-status="no-order">X</button>
                    </div>
                `;
                
                memberList.appendChild(memberItem);
                input.value = '';
                updateCounts();
            }
        }

        // Enter 키로 멤버 추가
        document.addEventListener('keypress', function(e) {
            if (e.key === 'Enter' && e.target.tagName === 'INPUT') {
                const day = e.target.getAttribute('data-day');
                addMember(day);
            }
        });

        // 카운트 업데이트 함수
        function updateCounts() {
            const days = ['monday', 'tuesday', 'wednesday', 'thursday', 'friday'];
            
            days.forEach(day => {
                const dayList = document.querySelector(`div[data-day="${day}"]`);
                const orderCount = dayList.querySelectorAll('.status-btn.order.active').length;
                document.getElementById(`${day}-count`).textContent = orderCount;
            });
        }

        // 초기 카운트 설정
        updateCounts();
    </script>
</body>
</html>
