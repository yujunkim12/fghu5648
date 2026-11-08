<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>카카오톡 스타일 메신저</title>
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Noto Sans KR', sans-serif; }
    body { display: flex; height: 100vh; width: 100vw; background-color: #f0f0f0; overflow: hidden; }

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

    /* 사이드바 (너비 확대 및 하단 버튼 배치) */
    .sidebar { width: 340px; background-color: #ffe812; display: flex; flex-direction: column; border-right: 1px solid #ddd; height: 100vh; }
    .sidebar-header { padding: 18px; font-weight: bold; font-size: 16px; background-color: #f7e314; display: flex; justify-content: space-between; align-items: center; }
    
    .room-list { flex: 1; overflow-y: auto; }
    .room-item { padding: 15px; border-bottom: 1px solid rgba(0,0,0,0.05); cursor: pointer; display: flex; justify-content: space-between; align-items: center; }
    .room-item:hover { background-color: #f5dc00; }
    .room-name { font-weight: bold; font-size: 15px; }
    .room-info { font-size: 12px; color: #666; margin-top: 3px; }

    /* 하단에 배치된 메뉴 버튼 영역 */
    .sidebar-footer-menu { padding: 12px 15px; display: flex; gap: 8px; background-color: #fae100; border-top: 1px solid #e0c800; }
    .sidebar-footer-menu button { flex: 1; padding: 10px; font-size: 12px; font-weight: bold; border: none; border-radius: 6px; background: white; cursor: pointer; box-shadow: 0 1px 3px rgba(0,0,0,0.1); }
    .sidebar-footer-menu button:hover { background: #f0f0f0; }

    /* 메인 채팅창 (크기 대폭 확장) */
    .chat-container { flex: 1; display: flex; flex-direction: column; background-color: #bacee0; height: 100vh; }
    .chat-header { padding: 18px 20px; background-color: white; border-bottom: 1px solid #ddd; display: flex; justify-content: space-between; align-items: center; }
    .chat-header h3 { font-size: 18px; }
    .chat-messages { flex: 1; padding: 20px; overflow-y: auto; display: flex; flex-direction: column; gap: 12px; }
    
    /* 메시지 말풍선 크기 키움 */
    .message { max-width: 65%; padding: 10px 14px; border-radius: 12px; font-size: 15px; line-height: 1.4; position: relative; }
    .message.received { background-color: white; align-self: flex-start; }
    .message.sent { background-color: #ffe812; align-self: flex-end; }
    .sender-name { font-size: 12px; color: #555; margin-bottom: 4px; font-weight: bold; }

    /* 하단 입력창 크기 확대 */
    .chat-input-area { padding: 15px 20px; background-color: white; display: flex; gap: 12px; border-top: 1px solid #ddd; }
    .chat-input-area input { flex: 1; padding: 12px 15px; border: 1px solid #ddd; border-radius: 6px; outline: none; font-size: 14px; }
    .chat-input-area button { padding: 12px 25px; background-color: #ffe812; border: none; border-radius: 6px; cursor: pointer; font-weight: bold; font-size: 14px; }
    .chat-input-area button:hover { background-color: #f5dc00; }

    /* 공통 버튼 & 모달 스타일 */
    .action-btn { background-color: #333; color: white; border: none; padding: 8px 12px; border-radius: 4px; cursor: pointer; font-size: 12px; }
    .modal { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.5); justify-content: center; align-items: center; z-index: 1000; }
    .modal-content { background: white; padding: 24px; border-radius: 10px; width: 420px; display: flex; flex-direction: column; gap: 14px; }
    .ban-options { display: flex; gap: 10px; }
    .ban-options button { flex: 1; padding: 10px; border: 1px solid #ccc; background: #f9f9f9; cursor: pointer; border-radius: 4px; font-weight: bold; }
    .ban-options button:hover { background: #e0e0e0; }
    
    .admin-only-btn { font-size: 10px; color: red; margin-left: 6px; cursor: pointer; display: inline-block; background: none; border: none; font-weight: bold; }
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

  <!-- 왼쪽: 사이드바 -->
  <div class="sidebar">
    <div class="sidebar-header">
      <span id="currentUserDisplay">채팅 목록</span>
      <button class="action-btn" onclick="openSettingsModal()">⚙️ 설정</button>
    </div>
    
    <!-- 채팅방 목록 영역 -->
    <div class="room-list" id="roomListContainer">
      <div class="room-item" onclick="enterRoom('자유 수다방')">
        <div>
          <div class="room-name">자유 수다방</div>
          <div class="room-info" id="info-자유 수다방">그룹 • 참여 2명</div>
        </div>
      </div>
    </div>

    <!-- 하단에 배치된 메뉴 버튼들 -->
    <div class="sidebar-footer-menu">
      <button onclick="openCreateRoomModal()">➕ 채팅방 만들기</button>
      <button onclick="openInviteModal()">👥 친구 초대</button>
    </div>
  </div>

  <!-- 오른쪽: 확장된 메인 채팅창 -->
  <div class="chat-container">
    <div class="chat-header">
      <h3 id="currentRoomTitle">자유 수다방</h3>
      <span id="currentRoomParticipants" style="font-size: 13px; color: #666;">참여 2명</span>
    </div>

    <div class="chat-messages" id="chatMessages">
      <div class="message sent">
        네 반갑습니다! 자유 수다방에 오신 것을 환영합니다.
      </div>
    </div>

    <div class="chat-input-area">
      <input type="text" id="messageInput" placeholder="메시지를 입력하세요..." onkeypress="if(event.key === 'Enter') sendMessage()">
      <button onclick="sendMessage()">전송</button>
    </div>
  </div>

  <!-- 모달 1: 설정 및 관리자 모드 -->
  <div class="modal" id="settingsModal">
    <div class="modal-content">
      <h3>⚙️ 설정 및 관리자 모드</h3>
      <hr style="border: 0; border-top: 1px solid #eee;">
      
      <label style="font-size: 13px; font-weight: bold;">🔒 관리자 비밀번호 입력</label>
      <input type="password" id="adminPassword" placeholder="비밀번호 입력" style="padding: 8px; border: 1px solid #ccc; border-radius: 4px;">
      <button onclick="verifyAdmin()" style="padding: 8px; background: #333; color: white; border: none; border-radius: 4px; cursor: pointer;">인증하기</button>
      
      <div id="adminPanel" style="display: none; background: #f0f0f0; padding: 10px; border-radius: 4px; font-size: 12px;">
        <strong style="color: green;">[관리자 인증 완료]</strong>
        <p style="margin-top: 5px;">전체 채팅방 내역 조회가 가능합니다.</p>
        
        <div style="margin-top: 8px; max-height: 120px; overflow-y: auto; background: white; padding: 8px; border: 1px solid #ddd;">
          <strong>📁 [전체 채팅방 통합 로그]</strong>
          <p style="margin-top: 4px; color: #333;">• [자유방] 김철수: 테스트 메시지입니다.</p>
        </div>
      </div>

      <hr style="border: 0; border-top: 1px solid #eee; margin-top: 5px;">
      <button onclick="handleLogout()" style="padding: 8px; background: #ff4d4d; color: white; border: none; border-radius: 4px; cursor: pointer;">로그아웃 (계정 초기화)</button>
      <button onclick="closeModal('settingsModal')" style="padding: 6px;">닫기</button>
    </div>
  </div>

  <!-- 모달 2: 사용자 정지 인증 처리 -->
  <div class="modal" id="banModal">
    <div class="modal-content">
      <h3>🚫 사용자 이용 정지 (관리자 인증)</h3>
      <p>제재 대상: <strong id="banTargetUser">-</strong></p>
      
      <div id="banAuthStep">
        <label style="font-size: 12px; font-weight: bold;">🔒 관리자 비밀번호 입력:</label>
        <div style="display: flex; gap: 5px; margin-top: 5px;">
          <input type="password" id="banAdminPassword" placeholder="비밀번호 입력" style="flex: 1; padding: 8px; border: 1px solid #ccc; border-radius: 4px;">
          <button onclick="checkBanPassword()" style="padding: 8px 12px; background: #333; color: white; border: none; border-radius: 4px; cursor: pointer;">확인</button>
        </div>
      </div>

      <div id="banActionStep" style="display: none;">
        <p style="color: green; font-size: 12px; margin-bottom: 5px;">✓ 인증 완료. 정지 기간을 선택하세요.</p>
        <div class="ban-options">
          <button onclick="applyBan(1)">1일 정지</button>
          <button onclick="applyBan(3)">3일 정지</button>
          <button onclick="applyBan(5)">5일 정지</button>
        </div>
      </div>

      <button onclick="closeModal('banModal')" style="padding: 5px; margin-top: 10px;">취소</button>
    </div>
  </div>

  <!-- 모달 3: 채팅방 만들기 -->
  <div class="modal" id="createRoomModal">
    <div class="modal-content">
      <h3>➕ 새로운 채팅방 만들기</h3>
      <label style="font-size: 12px;">채팅방 이름 (또는 상대방 이름):</label>
      <input type="text" id="newRoomNameInput" placeholder="예: 개발자 스터디룸" style="padding: 8px; border: 1px solid #ccc; border-radius: 4px;">
      
      <label style="font-size: 12px; margin-top: 5px;">방 유형 선택:</label>
      <div class="ban-options">
        <button onclick="createRoom('1:1 채팅')">1:1 채팅</button>
        <button onclick="createRoom('그룹 단톡방')">단체 대화방</button>
      </div>
      <button onclick="closeModal('createRoomModal')" style="padding: 6px; margin-top: 5px;">취소</button>
    </div>
  </div>

  <!-- 모달 4: 회원만 친구 초대 가능 -->
  <div class="modal" id="inviteModal">
    <div class="modal-content">
      <h3>👥 친구 검색 및 초대</h3>
      <p style="font-size: 12px; color: #666;">현재 방: <strong id="inviteTargetRoomName">자유 수다방</strong></p>
      
      <label style="font-size: 12px;">가입되어 있는 친구 닉네임 검색:</label>
      <div style="display: flex; gap: 5px;">
        <input type="text" id="friendSearchInput" placeholder="회원 닉네임 입력 (예: 김철수)" style="flex: 1; padding: 8px; border: 1px solid #ccc; border-radius: 4px;">
        <button onclick="searchAndInviteFriend()" style="padding: 8px 12px; background: #ffe812; border: none; border-radius: 4px; font-weight: bold; cursor: pointer;">초대</button>
      </div>
      <div id="searchResultArea" style="font-size: 12px; margin-top: 5px;"></div>

      <button onclick="closeModal('inviteModal')" style="padding: 6px; margin-top: 10px;">닫기</button>
    </div>
  </div>

  <script>
    let isAdmin = false;
    let selectedUserForBan = '';
    let currentActiveRoom = '자유 수다방';
    
    let registeredUsers = ['김철수', 'User1', 'User2'];

    let chatRooms = {
      '자유 수다방': { type: '그룹', participants: ['김철수'] }
    };

    window.onload = function() {
      const savedUser = localStorage.getItem('chat_nickname');
      if (savedUser) {
        document.getElementById('authScreen').style.display = 'none';
        document.getElementById('currentUserDisplay').textContent = `${savedUser} 님`;
        
        if (!registeredUsers.includes(savedUser)) registeredUsers.push(savedUser);
        if (!chatRooms['자유 수다방'].participants.includes(savedUser)) {
          chatRooms['자유 수다방'].participants.push(savedUser);
        }
        enterRoom('자유 수다방');
      }
    };

    function handleRegister() {
      const nickname = document.getElementById('nicknameInput').value.trim();
      if (!nickname) {
        alert('닉네임을 입력해주세요!');
        return;
      }

      const isDuplicate = registeredUsers.some(u => u.toLowerCase() === nickname.toLowerCase());
      if (isDuplicate) {
        alert(`'${nickname}'은 이미 누군가가 사용 중인 닉네임입니다. 다른 이름을 사용해 주세요.`);
        return;
      }

      localStorage.setItem('chat_nickname', nickname);
      registeredUsers.push(nickname);
      chatRooms['자유 수다방'].participants.push(nickname);

      alert(`${nickname}님 환영합니다! 회원가입이 완료되었습니다.`);
      document.getElementById('authScreen').style.display = 'none';
      document.getElementById('currentUserDisplay').textContent = `${nickname} 님`;
      enterRoom('자유 수다방');
    }

    function handleLogout() {
      if (confirm('정말 로그아웃 하시겠습니까? 저장된 계정이 초기화됩니다.')) {
        localStorage.removeItem('chat_nickname');
        location.reload();
      }
    }

    function enterRoom(roomName) {
      currentActiveRoom = roomName;
      document.getElementById('currentRoomTitle').textContent = roomName;
      
      const pList = chatRooms[roomName] ? chatRooms[roomName].participants : [];
      document.getElementById('currentRoomParticipants').textContent = `참여 ${pList.length}명 (${pList.join(', ')})`;
    }

    function openCreateRoomModal() {
      document.getElementById('newRoomNameInput').value = '';
      document.getElementById('createRoomModal').style.display = 'flex';
    }

    function createRoom(type) {
      const roomName = document.getElementById('newRoomNameInput').value.trim();
      if (!roomName) {
        alert('채팅방 이름을 입력해주세요!');
        return;
      }

      const isRoomDuplicate = Object.keys(chatRooms).some(name => name.toLowerCase() === roomName.toLowerCase());
      if (isRoomDuplicate) {
        alert(`'${roomName}' 이름의 채팅방이 이미 존재합니다.`);
        return;
      }

      const currentUser = localStorage.getItem('chat_nickname') || '나';
      chatRooms[roomName] = { type: type, participants: [currentUser] };

      const container = document.getElementById('roomListContainer');
      const roomDiv = document.createElement('div');
      roomDiv.className = 'room-item';
      roomDiv.onclick = () => enterRoom(roomName);
      roomDiv.innerHTML = `
        <div>
          <div class="room-name">${roomName}</div>
          <div class="room-info" id="info-${roomName}">${type} • 참여 1명</div>
        </div>
      `;
      container.appendChild(roomDiv);

      alert(`'${roomName}' (${type}) 방이 생성되었습니다!`);
      closeModal('createRoomModal');
      enterRoom(roomName);
    }

    function openInviteModal() {
      document.getElementById('inviteTargetRoomName').textContent = currentActiveRoom;
      document.getElementById('friendSearchInput').value = '';
      document.getElementById('searchResultArea').textContent = '';
      document.getElementById('inviteModal').style.display = 'flex';
    }

    function searchAndInviteFriend() {
      const friendName = document.getElementById('friendSearchInput').value.trim();
      const resultArea = document.getElementById('searchResultArea');
      const currentUser = localStorage.getItem('chat_nickname');

      if (!friendName) {
        alert('초대할 회원의 닉네임을 입력하세요.');
        return;
      }

      if (friendName === currentUser) {
        resultArea.innerHTML = `<span style="color: red;">자기 자신은 초대할 수 없습니다.</span>`;
        return;
      }

      const isRegistered = registeredUsers.some(user => user.toLowerCase() === friendName.toLowerCase());
      if (!isRegistered) {
        resultArea.innerHTML = `<span style="color: red;">🚫 '${friendName}' 님은 가입되어 있지 않은 회원입니다. (회원만 초대 가능)</span>`;
        return;
      }

      if (chatRooms[currentActiveRoom].participants.includes(friendName)) {
        resultArea.innerHTML = `<span style="color: red;">'${friendName}' 님은 이미 이 방에 참여 중입니다.</span>`;
        return;
      }

      chatRooms[currentActiveRoom].participants.push(friendName);
      resultArea.innerHTML = `<span style="color: green;">🎉 가입된 회원 '${friendName}' 님을 초대했습니다!</span>`;
      
      const pCount = chatRooms[currentActiveRoom].participants.length;
      const typeStr = chatRooms[currentActiveRoom].type;
      const infoElem = document.getElementById(`info-${currentActiveRoom}`);
      if (infoElem) infoElem.textContent = `${typeStr} • 참여 ${pCount}명`;

      enterRoom(currentActiveRoom);
    }

    function triggerBanAuth(username) {
      selectedUserForBan = username;
      document.getElementById('banTargetUser').textContent = username;
      document.getElementById('banAdminPassword').value = '';
      
      document.getElementById('banAuthStep').style.display = 'block';
      document.getElementById('banActionStep').style.display = 'none';
      
      document.getElementById('banModal').style.display = 'flex';
    }

    function checkBanPassword() {
      const inputPw = document.getElementById('banAdminPassword').value;

      if (inputPw === '1217') {
        document.getElementById('banAuthStep').style.display = 'none';
        document.getElementById('banActionStep').style.display = 'block';
      } else {
        alert('관리자 비밀번호가 틀렸습니다.');
      }
    }

    function applyBan(days) {
      alert(`[정지 승인 완료] ${selectedUserForBan} 님에게 ${days}일 이용 정지가 적용되었습니다.`);
      closeModal('banModal');
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

    function closeModal(modalId) {
      document.getElementById(modalId).style.display = 'none';
    }
  </script>
</body>
</html>
