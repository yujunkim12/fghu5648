<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>카카오톡 스타일 메신저</title>
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Noto Sans KR', sans-serif; }
    body { display: flex; height: 100vh; width: 100vw; background-color: #f0f0f0; overflow: hidden; }

    /* 최초 회원가입 화면 */
    #authScreen {
      position: fixed; top: 0; left: 0; width: 100%; height: 100%;
      background-color: #ffe812; z-index: 9999;
      display: flex; justify-content: center; align-items: center;
    }
    .auth-box {
      background: white; padding: 30px; border-radius: 12px; width: 350px;
      display: flex; flex-direction: column; gap: 15px; box-shadow: 0 4px 10px rgba(0,0,0,0.1);
    }
    .auth-box h2 { text-align: center; color: #333; }
    .auth-box input { padding: 12px; border: 1px solid #ddd; border-radius: 6px; outline: none; }
    .auth-box button { padding: 12px; background-color: #ffe812; border: none; border-radius: 6px; font-weight: bold; cursor: pointer; }
    .auth-box button:hover { background-color: #f5dc00; }

    /* 왼쪽 고정 메뉴 바 */
    .left-menu-bar { 
      width: 240px; background-color: #2b2b2b; display: flex; flex-direction: column; 
      height: 100vh; border-right: 1px solid #1a1a1a; flex-shrink: 0;
    }
    .menu-header-title {
      padding: 20px 15px 5px 15px;
    }

    /* 하단 버튼 영역을 맨 밑으로 밀착 */
    .menu-bottom-container { 
      margin-top: auto; 
      padding: 15px 12px; 
      display: flex; 
      flex-direction: column; 
      gap: 8px; 
      border-top: 1px solid #3d3d3d;
    }

    .menu-btn {
      background-color: #3d3d3d; color: white; border: none; padding: 14px; 
      border-radius: 8px; font-size: 15px; font-weight: bold; text-align: left; 
      cursor: pointer; transition: 0.2s; display: flex; align-items: center; gap: 10px;
      width: 100%;
    }
    .menu-btn:hover, .menu-btn.active { background-color: #ffe812; color: #000; }

    /* 우측 메인 콘텐츠 패널 영역 */
    .main-panel-area { flex: 1; display: flex; height: 100vh; background-color: #fffbe9; position: relative; overflow: hidden; }

    .content-tab { display: none; width: 100%; height: 100%; flex-direction: column; background-color: #fff; }
    .content-tab.active { display: flex; }

    .tab-header { padding: 20px 25px; background-color: #f7e314; font-size: 20px; font-weight: bold; border-bottom: 1px solid #e0c800; display: flex; justify-content: space-between; align-items: center; }

    /* 대화방 및 친구 목록 스타일 */
    .list-container { flex: 1; overflow-y: auto; }
    .list-item { padding: 18px 25px; border-bottom: 1px solid #eee; cursor: pointer; display: flex; align-items: center; gap: 15px; }
    .list-item:hover { background-color: #f9f9f9; }
    .item-avatar { width: 45px; height: 45px; border-radius: 50%; background-color: #ddd; display: flex; justify-content: center; align-items: center; overflow: hidden; font-size: 20px; }
    .item-avatar img { width: 100%; height: 100%; object-fit: cover; }
    .item-name { font-size: 16px; font-weight: bold; color: #333; }
    .item-sub { font-size: 13px; color: #777; margin-top: 3px; }

    /* 프로필 탭 스타일 */
    .profile-content-box { padding: 40px; display: flex; flex-direction: column; align-items: center; justify-content: center; flex: 1; gap: 15px; }
    .profile-big-avatar { width: 120px; height: 120px; border-radius: 50%; background-color: #ffe812; display: flex; justify-content: center; align-items: center; font-size: 50px; border: 3px solid #333; overflow: hidden; cursor: pointer; position: relative; }
    .profile-big-avatar img { width: 100%; height: 100%; object-fit: cover; }
    .profile-nickname-text { font-size: 24px; font-weight: bold; }
    .profile-status-text { font-size: 15px; color: #666; }

    /* 개별 채팅방 뷰 */
    #chatRoomView { display: none; flex-direction: column; flex: 1; height: 100vh; background-color: #bacee0; }
    #chatRoomView.active { display: flex; }

    .chat-room-header { padding: 18px 25px; background-color: white; border-bottom: 1px solid #ddd; display: flex; justify-content: space-between; align-items: center; }
    .chat-messages-box { flex: 1; padding: 20px; overflow-y: auto; display: flex; flex-direction: column; gap: 12px; }

    .message { max-width: 65%; padding: 12px 16px; border-radius: 12px; font-size: 15px; line-height: 1.4; position: relative; }
    .message.received { background-color: white; align-self: flex-start; }
    .message.sent { background-color: #ffe812; align-self: flex-end; }
    .sender-name { font-size: 12px; color: #555; margin-bottom: 4px; font-weight: bold; }

    .chat-bottom-input-area { padding: 15px 25px; background-color: white; display: flex; gap: 15px; border-top: 1px solid #ddd; align-items: center; }
    .chat-bottom-input-area input { flex: 1; padding: 14px 18px; border: 1px solid #ddd; border-radius: 8px; outline: none; font-size: 15px; }
    .send-btn-style { padding: 14px 28px; background-color: #ffe812; border: none; border-radius: 8px; cursor: pointer; font-weight: bold; font-size: 15px; }
    .send-btn-style:hover { background-color: #f5dc00; }

    /* 공통 모달 팝업 창 */
    .modal { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.5); justify-content: center; align-items: center; z-index: 10000; }
    .modal-content { background: white; padding: 25px; border-radius: 12px; width: 420px; display: flex; flex-direction: column; gap: 15px; box-shadow: 0 4px 15px rgba(0,0,0,0.2); }
    .ban-options { display: flex; gap: 10px; }
    .ban-options button { flex: 1; padding: 10px; border: 1px solid #ccc; background: #f9f9f9; cursor: pointer; border-radius: 6px; font-weight: bold; }
  </style>
</head>
<body>

  <!-- 최초 회원가입 화면 -->
  <div id="authScreen">
    <div class="auth-box">
      <h2>💬 간편 회원가입</h2>
      <p style="font-size: 12px; color: #666; text-align: center;">사용할 닉네임을 입력하세요.</p>
      <input type="text" id="nicknameInput" placeholder="사용할 닉네임 입력">
      <button onclick="handleRegister()">가입하고 시작하기</button>
    </div>
  </div>

  <!-- 왼쪽 고정 메뉴 바 -->
  <div class="left-menu-bar">
    <div class="menu-header-title">
      <h3 style="color: #ffe812; font-size: 18px;">카카오톡 메뉴</h3>
    </div>

    <!-- 하단 메뉴 버튼 영역 -->
    <div class="menu-bottom-container">
      <button class="menu-btn" onclick="openCreateRoomModal()">➕ 채팅창 생성하기</button>
      <button class="menu-btn" onclick="openInviteModal()">👥 친구 추가(초대)</button>
      <button class="menu-btn active" id="btnTabChat" onclick="switchMainTab('tabChatList')">💬 대화창 목록</button>
      <button class="menu-btn" id="btnTabFriends" onclick="switchMainTab('tabFriendsList')">👥 친구 목록</button>
      <button class="menu-btn" id="btnTabProfile" onclick="switchMainTab('tabProfile')">👤 프로필</button>
      
      <hr style="border: 0; border-top: 1px solid #444; margin: 4px 0;">
      
      <button class="menu-btn" onclick="openSettingsModal()" style="background-color: #444; font-size: 14px;">⚙️ 관리자 설정</button>
      <button class="menu-btn" onclick="handleLogout()" style="background-color: #a83232; font-size: 14px;">🚪 로그아웃</button>
    </div>
  </div>

  <!-- 우측 메인 콘텐츠 패널 영역 -->
  <div class="main-panel-area">

    <!-- 1. 대화창 목록 탭 -->
    <div class="content-tab active" id="tabChatList">
      <div class="tab-header"><span>💬 대화방 목록</span></div>
      <div class="list-container" id="roomListContainer"></div>
    </div>

    <!-- 2. 친구 목록 탭 -->
    <div class="content-tab" id="tabFriendsList">
      <div class="tab-header"><span>👥 추가된 친구 목록</span></div>
      <div class="list-container" id="friendsListContainer">
        <div style="padding: 20px; text-align: center; color: #777;">추가된 친구가 없습니다. '친구 추가'를 통해 친구를 등록해보세요![cite: 1]</div>
      </div>
    </div>

    <!-- 3. 프로필 탭 -->
    <div class="content-tab" id="tabProfile">
      <div class="tab-header"><span>👤 내 프로필</span></div>
      <div class="profile-content-box">
        <div class="profile-big-avatar" id="profileImgContainer" onclick="document.getElementById('galleryInput').click()" title="클릭해서 사진 변경">
          <span id="profileEmoji">😊</span>
          <img id="profilePreviewImg" src="" style="display:none;" />
        </div>
        <input type="file" id="galleryInput" accept="image/*" style="display:none;" onchange="loadGalleryImage(event)">
        
        <div class="profile-nickname-text" id="myProfileName">-</div>
        <div class="profile-status-text" id="myStatusMsg">상태 메시지가 없습니다.</div>
        
        <div style="display: flex; gap: 10px; margin-top: 10px;">
          <button onclick="document.getElementById('galleryInput').click()" style="padding: 10px 20px; background: #ffe812; border: none; border-radius: 6px; font-weight: bold; cursor: pointer;">갤러리 사진 변경</button>
          <button onclick="editProfile()" style="padding: 10px 20px; background: #ddd; border: none; border-radius: 6px; font-weight: bold; cursor: pointer;">상태메시지 수정</button>
        </div>
      </div>
    </div>

    <!-- 4. 실제 개별 채팅방 전체화면 뷰 -->
    <div id="chatRoomView">
      <div class="chat-room-header">
        <div style="display: flex; align-items: center; gap: 12px;">
          <button onclick="closeChatRoom()" style="padding: 6px 12px; background: #ddd; border: none; border-radius: 4px; font-weight: bold; cursor: pointer;">◀ 목록으로</button>
          <h3 id="currentRoomTitle">대화방</h3>
        </div>
        <span id="currentRoomParticipants" style="font-size: 13px; color: #666;">참여 1명</span>
      </div>

      <div class="chat-messages-box" id="chatMessages"></div>

      <div class="chat-bottom-input-area">
        <input type="text" id="messageInput" placeholder="메시지를 입력하세요..." onkeypress="if(event.key === 'Enter') sendMessage()">
        <button class="send-btn-style" onclick="sendMessage()">보내기</button>
      </div>
    </div>

  </div>

  <!-- 모달 1: 친구 프로필 상세보기 팝업 -->
  <div class="modal" id="friendProfileModal">
    <div class="modal-content" style="align-items: center; text-align: center;">
      <h3>친구 프로필</h3>
      <div id="modalFriendAvatar" style="width: 100px; height: 100px; border-radius: 50%; background: #ddd; display: flex; align-items: center; justify-content: center; font-size: 40px; overflow: hidden; border: 2px solid #ffe812;">
        👤
      </div>
      <h2 id="modalFriendName" style="margin-top: 5px;">이름</h2>
      <p id="modalFriendStatus" style="color: #666; font-size: 14px;">상태 메시지</p>
      <button onclick="closeModal('friendProfileModal')" style="padding: 8px 20px; background: #ffe812; border: none; border-radius: 6px; font-weight: bold; cursor: pointer; margin-top: 10px;">확인</button>
    </div>
  </div>

  <!-- 모달 2: 관리자 설정 -->
  <div class="modal" id="settingsModal">
    <div class="modal-content">
      <h3>⚙️ 관리자 설정 (비밀번호 인증)</h3>
      <label style="font-size: 13px; font-weight: bold;">🔒 관리자 비밀번호</label>
      <input type="password" id="adminPassword" placeholder="비밀번호 입력" style="padding: 10px; border: 1px solid #ccc; border-radius: 6px;">
      <button onclick="verifyAdmin()" style="padding: 10px; background: #333; color: white; border: none; border-radius: 6px; font-weight: bold; cursor: pointer;">인증하기</button>
      
      <div id="adminPanel" style="display: none; background: #f0f0f0; padding: 12px; border-radius: 6px; font-size: 12px;">
        <strong style="color: green;">[관리자 인증 완료]</strong>
        <p style="margin-top: 5px;">전체 통합 로그 조회 및 유저 제재 권한이 활성화되었습니다.</p>
      </div>
      <button onclick="closeModal('settingsModal')" style="padding: 8px; background: #eee; border: none; border-radius: 6px; cursor: pointer; margin-top: 10px;">닫기</button>
    </div>
  </div>

  <!-- 모달 3: 채팅창 생성하기 -->
  <div class="modal" id="createRoomModal">
    <div class="modal-content">
      <h3>➕ 새로운 채팅창 생성하기</h3>
      <label style="font-size: 13px;">채팅방 이름:</label>
      <input type="text" id="newRoomNameInput" placeholder="예: 재밌는 방" style="padding: 10px; border: 1px solid #ccc; border-radius: 6px;">
      
      <label style="font-size: 13px; margin-top: 5px;">대화할 상대방 (가입된 닉네임):</label>
      <input type="text" id="targetFriendInput" placeholder="예: User1 (선택 사항)" style="padding: 10px; border: 1px solid #ccc; border-radius: 6px;">

      <label style="font-size: 13px; margin-top: 5px;">방 유형:</label>
      <div class="ban-options">
        <button onclick="createRoom('1:1 채팅')">1:1 채팅</button>
        <button onclick="createRoom('그룹 단톡방')">단체 대화방</button>
      </div>
      <button onclick="closeModal('createRoomModal')" style="padding: 8px; background: #eee; border: none; border-radius: 6px; cursor: pointer;">취소</button>
    </div>
  </div>

  <!-- 모달 4: 친구 추가 (초대) -->
  <div class="modal" id="inviteModal">
    <div class="modal-content">
      <h3>👥 친구 추가 및 초대</h3>
      <p style="font-size: 12px; color: #666;">대상 채팅방: <strong id="inviteTargetRoomName">-</strong></p>
      
      <label style="font-size: 13px;">가입된 친구 닉네임 검색:</label>
      <div style="display: flex; gap: 8px;">
        <input type="text" id="friendSearchInput" placeholder="닉네임 입력" style="flex: 1; padding: 10px; border: 1px solid #ccc; border-radius: 6px;">
        <button onclick="searchAndInviteFriend()" style="padding: 10px 15px; background: #ffe812; border: none; border-radius: 6px; font-weight: bold; cursor: pointer;">추가</button>
      </div>
      <div id="searchResultArea" style="font-size: 13px; margin-top: 5px;"></div>

      <button onclick="closeModal('inviteModal')" style="padding: 8px; background: #eee; border: none; border-radius: 6px; cursor: pointer; margin-top: 10px;">닫기</button>
    </div>
  </div>

  <script>
    let isAdmin = false;
    let currentActiveRoom = '';
    
    let registeredUsers = JSON.parse(localStorage.getItem('chat_registered_users')) || ['User1', 'User2'];
    let chatRooms = JSON.parse(localStorage.getItem('chat_saved_rooms')) || {};

    let addedFriends = JSON.parse(localStorage.getItem('chat_added_friends')) || {
      'User1': { status: '안녕하세요! User1입니다.', avatar: '' },
      'User2': { status: '반갑습니다.', avatar: '' }
    };

    window.onload = function() {
      const savedUser = localStorage.getItem('chat_nickname');
      if (savedUser) {
        document.getElementById('authScreen').style.display = 'none';
        document.getElementById('myProfileName').textContent = savedUser;
        if (!registeredUsers.includes(savedUser)) {
          registeredUsers.push(savedUser);
          localStorage.setItem('chat_registered_users', JSON.stringify(registeredUsers));
        }
      }

      const savedStatus = localStorage.getItem('chat_status_msg');
      if (savedStatus) {
        document.getElementById('myStatusMsg').textContent = savedStatus;
      }

      const savedImage = localStorage.getItem('chat_profile_img');
      if (savedImage) {
        applyProfileImage(savedImage);
      }

      renderFriendsList();
      renderChatRoomsList();
    };

    function handleRegister() {
      const nickname = document.getElementById('nicknameInput').value.trim();
      if (!nickname) {
        alert('닉네임을 입력해주세요!');
        return;
      }

      const isDuplicate = registeredUsers.some(u => u.toLowerCase() === nickname.toLowerCase());
      if (isDuplicate) {
        alert(`'${nickname}'은(는) 이미 누군가 사용 중인 닉네임입니다. 다른 닉네임을 입력해 주세요.`);
        return;
      }

      registeredUsers.push(nickname);
      localStorage.setItem('chat_registered_users', JSON.stringify(registeredUsers));
      localStorage.setItem('chat_nickname', nickname);

      alert(`${nickname}님 환영합니다!`);
      document.getElementById('authScreen').style.display = 'none';
      document.getElementById('myProfileName').textContent = nickname;
      renderFriendsList();
      renderChatRoomsList();
    }

    function loadGalleryImage(event) {
      const file = event.target.files[0];
      if (file) {
        const reader = new FileReader();
        reader.onload = function(e) {
          const base64Image = e.target.result;
          localStorage.setItem('chat_profile_img', base64Image);[cite: 1]
          applyProfileImage(base64Image);
        };
        reader.readAsDataURL(file);
      }
    }

    function applyProfileImage(imgSrc) {
      const emojiElem = document.getElementById('profileEmoji');
      const imgElem = document.getElementById('profilePreviewImg');
      
      emojiElem.style.display = 'none';
      imgElem.style.display = 'block';
      imgElem.src = imgSrc;
    }

    function editProfile() {
      const currentMsg = document.getElementById('myStatusMsg').textContent;
      const newStatus = prompt('변경할 상태 메시지를 입력하세요:', currentMsg === '상태 메시지가 없습니다.' ? '' : currentMsg);
      if (newStatus !== null) {
        const finalMsg = newStatus.trim() || '상태 메시지가 없습니다.';
        document.getElementById('myStatusMsg').textContent = finalMsg;
        localStorage.setItem('chat_status_msg', finalMsg);[cite: 1]
      }
    }

    function switchMainTab(tabId) {
      closeChatRoom();

      document.querySelectorAll('.content-tab').forEach(el => el.classList.remove('active'));
      document.getElementById(tabId).classList.add('active');

      document.querySelectorAll('.menu-btn').forEach(el => el.classList.remove('active'));
      if (tabId === 'tabChatList') {
        document.getElementById('btnTabChat').classList.add('active');
        renderChatRoomsList();
      }
      if (tabId === 'tabFriendsList') document.getElementById('btnTabFriends').classList.add('active');
      if (tabId === 'tabProfile') document.getElementById('btnTabProfile').classList.add('active');
    }

    function renderFriendsList() {
      const container = document.getElementById('friendsListContainer');
      const friendNames = Object.keys(addedFriends);

      if (friendNames.length === 0) {
        container.innerHTML = `<div style="padding: 20px; text-align: center; color: #777;">추가된 친구가 없습니다.</div>`;
        return;
      }

      container.innerHTML = '';
      friendNames.forEach(fName => {
        const fData = addedFriends[fName];
        const item = document.createElement('div');
        item.className = 'list-item';
        item.onclick = function() { openFriendProfile(fName); };[cite: 1]
        
        let avatarContent = `👤`;
        if (fData.avatar) {
          avatarContent = `<img src="${fData.avatar}">`;
        }

        item.innerHTML = `
          <div class="item-avatar">${avatarContent}</div>
          <div>
            <div class="item-name">${fName}</div>
            <div class="item-sub">${fData.status}</div>
          </div>
        `;
        container.appendChild(item);
      });
    }

    function renderChatRoomsList() {
      const container = document.getElementById('roomListContainer');
      const roomNames = Object.keys(chatRooms);
      const currentUser = localStorage.getItem('chat_nickname') || '나';

      const myRooms = roomNames.filter(rName => chatRooms[rName].participants.includes(currentUser));

      if (myRooms.length === 0) {
        container.innerHTML = `<div style="padding: 20px; text-align: center; color: #777;">참여 중인 대화방이 없습니다. '채팅창 생성하기'로 방을 만들어보세요!</div>`;
        return;
      }

      container.innerHTML = '';
      myRooms.forEach(roomName => {
        const roomData = chatRooms[roomName];
        const lastMsgObj = roomData.messages && roomData.messages.length > 0 ? roomData.messages[roomData.messages.length - 1] : null;
        const lastMsgText = lastMsgObj ? `${lastMsgObj.sender}: ${lastMsgObj.text}` : '대화 내역이 없습니다.';

        const roomDiv = document.createElement('div');
        roomDiv.className = 'list-item';
        roomDiv.onclick = function() { enterRoom(roomName); };
        roomDiv.innerHTML = `
          <div class="item-avatar">💬</div>
          <div>
            <div class="item-name">${roomName}</div>
            <div class="item-sub" id="info-${roomName}">${lastMsgText} (${roomData.participants.length}명)</div>
          </div>
        `;
        container.appendChild(roomDiv);
      });
    }

    function openFriendProfile(fName) {
      const fData = addedFriends[fName];
      document.getElementById('modalFriendName').textContent = fName;
      document.getElementById('modalFriendStatus').textContent = fData.status;
      
      const avatarBox = document.getElementById('modalFriendAvatar');
      if (fData.avatar) {
        avatarBox.innerHTML = `<img src="${fData.avatar}" style="width:100%; height:100%; object-fit:cover;">`;
      } else {
        avatarBox.innerHTML = `👤`;
      }

      document.getElementById('friendProfileModal').style.display = 'flex';
    }

    function enterRoom(roomName) {
      currentActiveRoom = roomName;
      document.getElementById('currentRoomTitle').textContent = roomName;
      
      const roomData = chatRooms[roomName];
      const pList = roomData ? roomData.participants : [];
      document.getElementById('currentRoomParticipants').textContent = `참여 ${pList.length}명 (${pList.join(', ')})`;
      
      renderMessages();

      document.querySelectorAll('.content-tab').forEach(el => el.classList.remove('active'));
      document.getElementById('chatRoomView').classList.add('active');
    }

    function renderMessages() {
      const chatMessages = document.getElementById('chatMessages');
      chatMessages.innerHTML = '';

      const roomData = chatRooms[currentActiveRoom];
      const currentUser = localStorage.getItem('chat_nickname') || '나';

      if (!roomData || !roomData.messages || roomData.messages.length === 0) {
        chatMessages.innerHTML = `<div class="message received" style="background: #fff; text-align: center; width: 100%; color: #666;">'${currentActiveRoom}' 방에 입장했습니다. 대화를 시작해보세요!</div>`;
        return;
      }

      roomData.messages.forEach(msg => {
        const msgDiv = document.createElement('div');
        if (msg.sender === currentUser) {
          msgDiv.className = 'message sent';
          msgDiv.innerHTML = `<div class="sender-name">${msg.sender}</div>${msg.text}`;
        } else {
          msgDiv.className = 'message received';
          msgDiv.innerHTML = `<div class="sender-name">${msg.sender}</div>${msg.text}`;
        }
        chatMessages.appendChild(msgDiv);
      });

      chatMessages.scrollTop = chatMessages.scrollHeight;
    }

    function closeChatRoom() {
      document.getElementById('chatRoomView').classList.remove('active');
      document.getElementById('tabChatList').classList.add('active');
      currentActiveRoom = '';
    }

    function openCreateRoomModal() {
      document.getElementById('newRoomNameInput').value = '';
      document.getElementById('targetFriendInput').value = '';
      document.getElementById('createRoomModal').style.display = 'flex';
    }

    function createRoom(type) {
      const roomName = document.getElementById('newRoomNameInput').value.trim();
      const targetFriend = document.getElementById('targetFriendInput').value.trim();
      const currentUser = localStorage.getItem('chat_nickname') || '나';

      if (!roomName) {
        alert('채팅방 이름을 입력해주세요!');
        return;
      }
      if (chatRooms[roomName]) {
        alert(`'${roomName}' 이름의 방이 이미 존재합니다.`);
        return;
      }

      const participants = [currentUser];

      if (targetFriend) {
        const isRegistered = registeredUsers.some(u => u.toLowerCase() === targetFriend.toLowerCase());
        if (!isRegistered) {
          alert(`'${targetFriend}' 님은 가입되어 있지 않은 회원입니다.`);
          return;
        }
        if (targetFriend !== currentUser) {
          participants.push(targetFriend);
        }
      }

      chatRooms[roomName] = { 
        type: type, 
        participants: participants,
        messages: [] 
      };

      localStorage.setItem('chat_saved_rooms', JSON.stringify(chatRooms));
      closeModal('createRoomModal');

      switchMainTab('tabChatList');
      renderChatRoomsList();

      alert(`'${roomName}' 대화방이 생성되어 대화창 목록에 추가되었습니다!`);
    }

    function openInviteModal() {
      if (!currentActiveRoom) {
        alert('먼저 대화방에 입장해 주세요!');
        return;
      }
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
        alert('친구 닉네임을 입력하세요.');
        return;
      }
      if (friendName === currentUser) {
        resultArea.innerHTML = `<span style="color: red;">자기 자신은 추가할 수 없습니다.</span>`;
        return;
      }
      
      const isRegistered = registeredUsers.some(u => u.toLowerCase() === friendName.toLowerCase());
      if (!isRegistered) {
        resultArea.innerHTML = `<span style="color: red;">🚫 가입되지 않은 회원입니다.</span>`;
        return;
      }

      if (!addedFriends[friendName]) {
        addedFriends[friendName] = { status: '반갑습니다!', avatar: '' };
        localStorage.setItem('chat_added_friends', JSON.stringify(addedFriends));
      }

      if (chatRooms[currentActiveRoom].participants.includes(friendName)) {
        resultArea.innerHTML = `<span style="color: red;">이미 참여 중인 회원입니다.</span>`;
        return;
      }

      chatRooms[currentActiveRoom].participants.push(friendName);
      localStorage.setItem('chat_saved_rooms', JSON.stringify(chatRooms));

      renderFriendsList();
      renderChatRoomsList();

      resultArea.innerHTML = `<span style="color: green;">🎉 '${friendName}' 님을 대화 상대방으로 추가했습니다![cite: 1]</span>`;
      
      const pCount = chatRooms[currentActiveRoom].participants.length;
      document.getElementById('currentRoomParticipants').textContent = `참여 ${pCount}명 (${chatRooms[currentActiveRoom].participants.join(', ')})`;

      setTimeout(() => {
        closeModal('inviteModal');
      }, 1000);
    }

    function sendMessage() {
      if (!currentActiveRoom) return;
      const input = document.getElementById('messageInput');
      const text = input.value.trim();
      if (!text) return;

      const currentNick = localStorage.getItem('chat_nickname') || '나';

      if (!chatRooms[currentActiveRoom].messages) {
        chatRooms[currentActiveRoom].messages = [];
      }

      chatRooms[currentActiveRoom].messages.push({
        sender: currentNick,
        text: text,
        time: new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' })
      });

      localStorage.setItem('chat_saved_rooms', JSON.stringify(chatRooms));

      renderMessages();
      renderChatRoomsList();

      input.value = '';
    }

    function openSettingsModal() {
      document.getElementById('settingsModal').style.display = 'flex';
    }

    function verifyAdmin() {
      const inputPw = document.getElementById('adminPassword').value;
      if (inputPw === '1217') { 
        isAdmin = true;
        alert('관리자 인증 성공!');
        document.getElementById('adminPanel').style.display = 'block';
      } else {
        alert('비밀번호가 틀렸습니다.');
      }
    }

    function handleLogout() {
      if (confirm('정말 로그아웃 하시겠습니까?')) {
        localStorage.removeItem('chat_nickname');
        localStorage.removeItem('chat_status_msg');
        localStorage.removeItem('chat_profile_img');
        location.reload();
      }
    }

    function closeModal(modalId) {
      document.getElementById(modalId).style.display = 'none';
    }
  </script>
</body>
</html>
