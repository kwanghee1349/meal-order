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
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            border-radius: 15px;
            padding: 25px;
            border: 2px solid #e9ecef;
            transition: all 0.3s ease;
            box-shadow: 0 4px 12px rgba(0,0,0,0.1);
        }

        .day-card:hover {
            border-color: #667eea;
            box-shadow: 0 8px 25px rgba(102, 126, 234, 0.15);
            transform: translateY(-3px);
        }

        .day-title {
            font-size: 1.3em;
            font-weight: 700;
            margin-bottom: 15px;
            color: white;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .day-emoji {
            font-size: 1.5em;
        }

        .member-list {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 10px;
        }

        .member-item {
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 12px;
            background: white;
            border-radius: 10px;
            border: 2px solid #e9ecef;
            transition: all 0.2s ease;
            box-shadow: 0 2px 8px rgba(0,0,0,0.1);
            cursor: pointer;
            user-select: none;
        }

        .member-item:hover {
            border-color: #5a6fd8;
            box-shadow: 0 4px 15px rgba(102, 126, 234, 0.3);
            transform: translateY(-2px);
        }

        .member-item.long-pressing {
            background: #ffe6e6;
            border-color: #dc3545;
        }

        .delete-modal {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.5);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 1000;
        }

        .delete-modal-content {
            background: white;
            padding: 30px;
            border-radius: 15px;
            text-align: center;
            box-shadow: 0 10px 30px rgba(0,0,0,0.3);
            max-width: 300px;
            width: 90%;
        }

        .delete-modal h3 {
            color: #495057;
            margin-bottom: 15px;
            font-size: 1.2em;
        }

        .delete-modal p {
            color: #6c757d;
            margin-bottom: 20px;
        }

        .delete-modal-buttons {
            display: flex;
            gap: 10px;
            justify-content: center;
        }

        .delete-modal-btn {
            padding: 10px 20px;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-weight: 600;
            transition: all 0.2s ease;
        }

        .delete-modal-btn.confirm {
            background: #dc3545;
            color: white;
        }

        .delete-modal-btn.confirm:hover {
            background: #c82333;
        }

        .delete-modal-btn.cancel {
            background: #6c757d;
            color: white;
        }

        .delete-modal-btn.cancel:hover {
            background: #5a6268;
        }

        .member-name {
            font-weight: 600;
            color: #495057;
            font-size: 13px;
            flex: 1;
            margin-right: 5px;
            cursor: pointer;
            padding: 2px 4px;
            border-radius: 4px;
            transition: background-color 0.2s ease;
        }

        .member-name:hover {
            background-color: #f8f9fa;
        }

        .member-name-input {
            font-weight: 600;
            color: #495057;
            font-size: 13px;
            flex: 1;
            margin-right: 5px;
            border: 1px solid #667eea;
            border-radius: 4px;
            padding: 2px 4px;
            background: #f8f9fa;
        }

        .status-toggle {
            display: flex;
            gap: 3px;
        }

        .status-btn {
            width: 32px;
            height: 32px;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            font-size: 1em;
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
            gap: 8px;
            grid-column: 1 / -1;
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
            <h1>🍱 식사 포장 주문 현황</h1>
            <p>매일 누가 포장 주문하는지 체크해보세요! - by 이광희</p>
        </div>

        <div class="content">
            <div class="day-grid">
                <div class="day-card">
                    <div class="day-title">
                        <span class="day-emoji">🌅</span>
                        월요일
                    </div>
                    <div class="member-list" data-day="monday"></div>
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
                    <div class="member-list" data-day="tuesday"></div>
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
                    <div class="member-list" data-day="wednesday"></div>
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
                    <div class="member-list" data-day="thursday"></div>
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
                    <div class="member-list" data-day="friday"></div>
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
                        <div class="summary-count" id="monday-count">0</div>
                    </div>
                    <div class="summary-item">
                        <div class="summary-day">화</div>
                        <div class="summary-count" id="tuesday-count">0</div>
                    </div>
                    <div class="summary-item">
                        <div class="summary-day">수</div>
                        <div class="summary-count" id="wednesday-count">0</div>
                    </div>
                    <div class="summary-item">
                        <div class="summary-day">목</div>
                        <div class="summary-count" id="thursday-count">0</div>
                    </div>
                    <div class="summary-item">
                        <div class="summary-day">금</div>
                        <div class="summary-count" id="friday-count">0</div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <script>
        // 기본 멤버 목록
        const defaultMembers = ['이광희','강규호','김광수','김남열','김원교','김재강','마준열','박재홍','박정','안태균','유승국','이동현','이정민','정외섭','조양일','추봉구'];
        
        // 페이지 로드 시 모든 요일에 기본 멤버 추가
        document.addEventListener('DOMContentLoaded', function() {
            const days = ['monday', 'tuesday', 'wednesday', 'thursday', 'friday'];
            days.forEach(day => {
                defaultMembers.forEach(name => {
                    createMemberItem(day, name);
                });
            });
            updateCounts();
        });

        // 멤버 아이템 생성 함수
        function createMemberItem(day, name) {
            const memberList = document.querySelector(`div[data-day="${day}"]`);
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
        }

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

        // 이름 편집 기능
        document.addEventListener('click', function(e) {
            if (e.target.classList.contains('member-name')) {
                const nameSpan = e.target;
                const currentName = nameSpan.textContent;
                
                // input 요소 생성
                const input = document.createElement('input');
                input.type = 'text';
                input.value = currentName;
                input.className = 'member-name-input';
                
                // span을 input으로 교체
                nameSpan.parentNode.replaceChild(input, nameSpan);
                input.focus();
                input.select();
                
                // Enter 키 또는 포커스 잃을 때 저장
                function saveName() {
                    const newName = input.value.trim();
                    if (newName) {
                        const newSpan = document.createElement('span');
                        newSpan.className = 'member-name';
                        newSpan.textContent = newName;
                        input.parentNode.replaceChild(newSpan, input);
                    } else {
                        // 빈 이름이면 원래 이름으로 복원
                        const newSpan = document.createElement('span');
                        newSpan.className = 'member-name';
                        newSpan.textContent = currentName;
                        input.parentNode.replaceChild(newSpan, input);
                    }
                }
                
                input.addEventListener('keypress', function(e) {
                    if (e.key === 'Enter') {
                        saveName();
                    }
                });
                
                input.addEventListener('blur', saveName);
            }
        });

        // 멤버 추가 기능
        function addMember(day) {
            const input = document.querySelector(`input[data-day="${day}"]`);
            const name = input.value.trim();
            
            if (name) {
                createMemberItem(day, name);
                input.value = '';
                updateCounts();
            }
        }

        // 길게 누르기 기능
        let longPressTimer;
        let longPressTarget;

        document.addEventListener('mousedown', function(e) {
            const memberItem = e.target.closest('.member-item');
            if (memberItem && !e.target.classList.contains('status-btn') && !e.target.classList.contains('member-name')) {
                longPressTarget = memberItem;
                memberItem.classList.add('long-pressing');
                longPressTimer = setTimeout(() => {
                    showDeleteModal(memberItem);
                }, 800); // 0.8초 길게 누르기
            }
        });

        document.addEventListener('mouseup', function(e) {
            if (longPressTimer) {
                clearTimeout(longPressTimer);
                if (longPressTarget) {
                    longPressTarget.classList.remove('long-pressing');
                }
                longPressTarget = null;
            }
        });

        document.addEventListener('mouseleave', function(e) {
            if (longPressTimer) {
                clearTimeout(longPressTimer);
                if (longPressTarget) {
                    longPressTarget.classList.remove('long-pressing');
                }
                longPressTarget = null;
            }
        });

        // 터치 이벤트 (모바일)
        document.addEventListener('touchstart', function(e) {
            const memberItem = e.target.closest('.member-item');
            if (memberItem && !e.target.classList.contains('status-btn') && !e.target.classList.contains('member-name')) {
                longPressTarget = memberItem;
                memberItem.classList.add('long-pressing');
                longPressTimer = setTimeout(() => {
                    showDeleteModal(memberItem);
                }, 800);
            }
        });

        document.addEventListener('touchend', function(e) {
            if (longPressTimer) {
                clearTimeout(longPressTimer);
                if (longPressTarget) {
                    longPressTarget.classList.remove('long-pressing');
                }
                longPressTarget = null;
            }
        });

        // 삭제 확인 모달 표시
        function showDeleteModal(memberItem) {
            const memberName = memberItem.querySelector('.member-name').textContent;
            
            const modal = document.createElement('div');
            modal.className = 'delete-modal';
            modal.innerHTML = `
                <div class="delete-modal-content">
                    <h3>삭제 확인</h3>
                    <p>"${memberName}"을(를) 삭제하시겠습니까?</p>
                    <div class="delete-modal-buttons">
                        <button class="delete-modal-btn confirm">예</button>
                        <button class="delete-modal-btn cancel">아니오</button>
                    </div>
                </div>
            `;
            
            document.body.appendChild(modal);
            
            // 확인 버튼
            modal.querySelector('.confirm').addEventListener('click', function() {
                memberItem.remove();
                document.body.removeChild(modal);
                updateCounts();
            });
            
            // 취소 버튼
            modal.querySelector('.cancel').addEventListener('click', function() {
                document.body.removeChild(modal);
            });
            
            // 모달 외부 클릭 시 닫기
            modal.addEventListener('click', function(e) {
                if (e.target === modal) {
                    document.body.removeChild(modal);
                }
            });
            
            if (longPressTarget) {
                longPressTarget.classList.remove('long-pressing');
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
    </script>
</body>
</html>
