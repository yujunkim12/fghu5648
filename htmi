HTML
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>카카오톡 스타일 메신저 (관리자 비밀번호 인증 포함)</title>
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Noto Sans KR', sans-serif; }
    body { display: flex; height: 100vh; background-color: #f0f0f0; }

    /* 회원가입/로그인 오버레이 화면 */
    #authScreen {
      position: fixed; top: 0; left: 0; width: 100%; height: 100%;
      background-color: #ffe812; z-index: 999;
      display: flex; justify-content: center; align-items: center;
    }
    .auth-box {
      background: white; padding: 30px; border-radius: 12px; width: 350px;
      display: flex; flex-direction: column; gap: 15px; box-shadow: 0 4px 10px rgba(0,0,0,0.1);
    }
    .auth-box h2 { text-align: center; color: #333; }
    .auth-box input { padding: 10px; border: 1px solid #ddd; border-radius: 6px; outline: none; }
    .auth-box button { padding: 10px; background-color: #ffe812; border: none; border-radius: 6px; font-weight: bold; cursor: pointer; }
    .auth-box button:hover { background-color: #f5dc00; }

    /* 사이드바 */
    .sidebar { width: 280px; background-color: #ffe812; display: flex; flex-direction: column; border-right: 1px solid #ddd; }
    .sidebar-header { padding: 15px; font-weight: bold; background-color: #f7e314; display: flex; justify-content: space-between; align-items: center; }
    .room-list { flex: 1; overflow-y: auto; }
    .room-item { padding: 12px 15px; border-bottom: 1px solid rgba(0,0,0,0.05); cursor: pointer; }
    .room-item:hover { background-color: #f5dc00; }

    /* 메인 채팅창 */
    .chat-container { flex: 1; display: flex; flex-direction: column; background-color: #bacee0; }
    .chat-header { padding: 15px; background-color: white; border-bottom: 1px solid #ddd; display: flex; justify-content: space-between; align-items: center; }
    .chat-messages { flex: 1; padding: 15px; overflow-y: auto; display: flex; flex-direction: column; gap: 10px; }
    
    /* 메시지 말풍선 */
    .message { max-width: 60%; padding: 8px 12px; border-radius: 10px; font-size: 14px; position: relative; }
    .message.received { background-color: white; align-self: flex-start; }
    .message.sent { background-color: #ffe812; align-self: flex-end; }
    .sender-name { font-size: 11px; color: #555; margin-bottom: 3px; }

    /* 입력창 */
    .chat-input-area { padding: 10px; background-color: white; display: flex; gap: 10px; }
    .chat-input-area input { flex: 1; padding: 10px; border: 1px solid #ddd; border-radius: 5px; outline: none; }
    .chat-input-area button { padding: 10px 20px; background-color: #ffe812; border: none; border-radius: 5px; cursor: pointer; font-weight: bold; }

    /* 버튼 & 모달 스타일 */
    .settings-btn { background-color: #333; color: white; border: none; padding: 6px 12px; border-radius: 4px; cursor: pointer; font-size: 12px; }
    .modal { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.5); justify-content: center; align-items: center; z-index: 1000; }
    .modal-content { background: white; padding: 20px; border-radius: 8px; width: 400px; display: flex; flex-direction: column; gap: 12px; }
    .ban-options { display: flex; gap: 10px; }
    .ban-options button { flex: 1; padding: 8px; border: 1px solid #ccc; background: #f9f9f9; cursor: pointer; border-radius: 4px; }
    .ban-options button:hover { background: #e0e0e0; }
    
    .admin-only-btn { font-size: 9px; color: red; margin-left: 5px; cursor: pointer; display: inline-block; }
  </style>
</head>
<body>

  <!-- 최초 회원가입 / 로그인 화면 -->
  <div id="authScreen">
    <div class="auth-box">
      <h2>💬 간편 회원가입</h2>
      <p style="font-size: 12px; color: #666; text-align: center;">사용할 닉네임을 입력하세요.</p>
      <input type="text" id="nicknameInput" placeholder="사용할 닉네임 입력">
      <button onclick="handleRegister()">가입하고 시작하기</button>
    </div>
  </div>

  <!-- 왼쪽: 채팅방 목록 -->
  <div class="sidebar">
    <div class="sidebar-header">
      <span id="currentUserDisplay">채팅 목록</span>
      <button class="settings-btn" onclick="openSettingsModal()">⚙️ 설정</button>
    </div>
    <div class="room-list">
      <div class="room-item"><strong>자유 수다방</strong></div>
      <div class="room-item"><strong>개발자 모임</strong></div>
    </div>
  </div>

  <!-- 오른쪽: 메인 채팅창 -->
  <div class="chat-container">
    <div class="chat-header">
      <h3>자유 수다방</h3>
      <span style="font-size: 12px; color: #666;">참여 3명</span>
    </div>

    <div class="chat-messages" id="chatMessages">
      <div class="message received">
        <div class="sender-name">홍길동 (User1)</div>
        안녕하세요! 오늘 날씨 좋네요.
        <button class="admin-only-btn" onclick="openBanModal('User1')">[제재]</button>
      </div>
      <div class="message sent">
        네 반갑습니다!
      </div>
    </div>

    <div class="chat-input-area">
      <input type="text" id="messageInput" placeholder="메시지를 입력하세요...">
      <button onclick="sendMessage()">전송</button>
    </div>
  </div>

  <!-- 모달 1: 설정 창 -->
  <div class="modal" id="settingsModal">
    <div class="modal-content">
      <h3>⚙️ 설정 및 관리자 모드</h3>
      <hr style="border: 0; border-top: 1px solid #eee;">
      
      <label style="font-size: 13px; font-weight: bold;">🔒 관리자 비밀번호 입력</label>
      <input type="password" id="adminPassword" placeholder="비밀번호 (1217)" style="padding: 8px; border: 1px solid #ccc; border-radius: 4px;">
      <button onclick="verifyAdmin()" style="padding: 8px; background: #333; color: white; border: none; border-radius: 4px; cursor: pointer;">인증하기</button>
      
      <div id="adminPanel" style="display: none; background: #f0f0f0; padding: 10px; border-radius: 4px; font-size: 12px;">
        <strong style="color: green;">[관리자 인증 완료]</strong>
        <p style="margin-top: 5px;">전체 채팅방 내역 조회가 가능합니다.</p>
        
        <div style="margin-top: 8px; max-height: 120px; overflow-y: auto; background: white; padding: 8px; border: 1px solid #ddd;">
          <strong>📁 [전체 채팅방 통합 로그]</strong>
          <p style="margin-top: 4px; color: #333;">• [자유방] 홍길동: 안녕하세요! 오늘 날씨 좋네요.</p>
          <p style="color: #333;">• [개발방] 김철수: 테스트 메시지입니다.</p>
        </div>
      </div>

      <hr style="border: 0; border-top: 1px solid #eee; margin-top: 5px;">
      <button onclick="handleLogout()" style="padding: 8px; background: #ff4d4d; color: white; border: none; border-radius: 4px; cursor: pointer;">로그아웃 (계정 초기화)</button>
      <button onclick="closeModal('settingsModal')" style="padding: 6px;">닫기</button>
    </div>
  </div>

  <!-- 모달 2: 사용자 정지 처리 (관리자 비밀번호 재확인 필수) -->
  <div class="modal" id="banModal">
    <div class="modal-content">
      <h3>🚫 사용자 이용 정지</h3>
      <p>대상 사용자: <strong id="banTargetUser">-</strong></p>
      
      <label style="font-size: 12px; font-weight: bold;">🔒 관리자 비밀번호 입력:</label>
      <input type="password" id="banAdminPassword" placeholder="비밀번호 (1217)" style="padding: 8px; border: 1px solid #ccc; border-radius: 4px;">

      <label style="font-size: 12px; margin-top: 5px;">정지 기간 선택:</label>
      <div class="ban-options">
        <button onclick="applyBan(1)">1일 정지</button>
        <button onclick="applyBan(3)">3일 정지</button>
        <button onclick="applyBan(5)">5일 정지</button>
      </div>
      <button onclick="closeModal('banModal')" style="padding: 5px; margin-top: 10px;">취소</button>
    </div>
  </div>

  <script>
    let isAdmin = false;
    let selectedUserForBan = '';

    window.onload = function() {
      const savedUser = localStorage.getItem('chat_nickname');
      if (savedUser) {
        document.getElementById('authScreen').style.display = 'none';
        document.getElementById('currentUserDisplay').textContent = `${savedUser} 님`;
      }
    };

    function handleRegister() {
      const nickname = document.getElementById('nicknameInput').value.trim();
      if (!nickname) {
        alert('닉네임을 입력해주세요!');
        return;
      }
      localStorage.setItem('chat_nickname', nickname);
      alert(`${nickname}님 환영합니다!`);
      document.getElementById('authScreen').style.display = 'none';
      document.getElementById('currentUserDisplay').textContent = `${nickname} 님`;
    }

    function handleLogout() {
      if (confirm('정말 로그아웃 하시겠습니까? 저장된 닉네임이 초기화됩니다.')) {
        localStorage.removeItem('chat_nickname');
        location.reload();
      }
    }

    function sendMessage() {
      const input = document.getElementById('messageInput');
      if (!input.value.trim()) return;

      const currentNick = localStorage.getItem('chat_nickname') || '나';
      const chatMessages = document.getElementById('chatMessages');
      const msgDiv = document.createElement('div');
      msgDiv.className = 'message sent';
      msgDiv.innerHTML = `<div class="sender-name">${currentNick}</div>${input.value}`;
      chatMessages.appendChild(msgDiv);

      input.value = '';
      chatMessages.scrollTop = chatMessages.scrollHeight;
    }

    function openSettingsModal() {
      document.getElementById('settingsModal').style.display = 'flex';
    }

    // 설정창 비밀번호 인증
    function verifyAdmin() {
      const inputPw = document.getElementById('adminPassword').value;
      if (inputPw === '1217') { 
        isAdmin = true;
        alert('관리자 인증에 성공했습니다.');
        document.getElementById('adminPanel').style.display = 'block';
      } else {
        alert('비밀번호가 올바르지 않습니다.');
      }
    }

    // 이용 정지 팝업 오픈
    function openBanModal(username) {
      selectedUserForBan = username;
      document.getElementById('banTargetUser').textContent = username;
      document.getElementById('banAdminPassword').value = ''; // 비밀번호 입력창 초기화
      document.getElementById('banModal').style.display = 'flex';
    }

    // 정지 버튼 클릭 시 관리자 비밀번호 검증 후 처리
    function applyBan(days) {
      const inputPw = document.getElementById('banAdminPassword').value;

      if (inputPw !== '1217') {
        alert('관리자 비밀번호(1217)가 올바르지 않습니다.');
        return;
      }

      alert(`[인증 성공] ${selectedUserForBan} 님에게 ${days}일 이용 정지가 적용되었습니다.`);
      closeModal('banModal');
    }

    function closeModal(modalId) {
      document.getElementById(modalId).style.display = 'none';
    }
  </script>
</body>
</html>
