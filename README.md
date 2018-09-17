# join.jsp
join.jsp 회원가입
```jsp
<div class="container">
	<div class="col=lg-4"></div>
	<div class="col-lg-4">
	<div class="jumbotron" style="padding-top: 20px;">
		<form method="post" action="joinAction.jsp">
			<h3 style="text-align: center;">회원가입 화면</h3>
			<div class="form-group">
				<input type="text" class="form-control" placeholder="아이디" name="userID" maxlength="20">
			</div>
			<div class="form-group">
				<input type="password" class="form-control" placeholder="비번" name="userPassword" maxlength="20">
			</div>
			<div class="form-group">
				<input type="text" class="form-control" placeholder="이름" name="userName" maxlength="20">
			</div>
			<div class ="form-group" style="text-align: center;">
			<div class = "btn-group" data-toggle="buttons">
				<label class = "btn btn-primary active">
					<input type="radio" name="userGender" autocomplete="off" value="남자" checked>남자
				</label>
				<label class = "btn btn-primary">
					<input type="radio" name="userGender" autocomplete="off" value="여자" checked>여자
				</label>
				</div>
				</div>
			<div class="form-group">
				<input type="email" class="form-control" placeholder="이메일" name="userEmail" maxlength="20">
			</div>
			<input type="submit" class="btn btn-primary form-control" value="회원가입">
		</form>	
</div>
</div>
```
```jsp
<div class = "btn-group" data-toggle="buttons">
				<label class = "btn btn-primary active">
					<input type="radio" name="userGender" autocomplete="off" value="남자" checked>남자
				</label>
				<label class = "btn btn-primary">
					<input type="radio" name="userGender" autocomplete="off" value="여자" checked>여자
				</label>
				</div>
				</div>
```
**버튼 그룹**<br>
버튼 그룹으로 한줄에 버튼을 그룹화 하고, 자바스크립트를 통해 이벤트 처리를 한다.<br>
**data-toggle="buttons"**<br>
버튼의 토글 상태를 위해 추가한 태그, **label class = "btn btn-primary active"** 처럼 active를 넣어 주면 기본 값으로 토글된 상태가 된다.<br>
**label** class = "btn btn-primary active"<br>
label을 준 이유는 부트스트랩에서 checkbox나 radio 버튼에 토글을 적용하기 위해서이다.<br>
**checked** 속성으로 수동으로 입력 받는다.

# UserDao.java
```java
public int join(User user) {
		String SQL = "INSERT INTO USER VALUES(?, ?, ?, ?, ?)";
		try {
			pstmt = conn.prepareStatement(SQL);
			pstmt.setString(1, user.getUserID());
			pstmt.setString(2, user.getUserPassword());
			pstmt.setString(3, user.getUserName());
			pstmt.setString(4, user.getUserGender());
			pstmt.setString(5, user.getUserEmail());
			return pstmt.executeUpdate();
		}catch(Exception e) {
			e.printStackTrace();
		}
		return -1;
	}
```
회원가입을 위해 db접근 메서드<br>
String SQL = "INSERT INTO USER VALUES(?, ?, ?, ?, ?)";<br>
INSERT 문은 반환되는 값이 무조건 0이상이기 때문에 return 값이 -1이라면 오류가 난 것이다.<br>
