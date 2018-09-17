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
***

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
pstmt = conn.prepareStatement(SQL); 문장의 prepareStatement를 통해 정해진 문장을 Connection conn으로 데이터베이스에 삽입하고<br>
그 결과를 PrepareStatement pstmt에 저장한다.<br>
그 후에 PrepareStatement.setString() 메서드를 통해 각각 해당하는 칼럼의 인덱스에 User.java에 들어있는 정보들을 db에 넣어준다.<br>
마지막으로 **return pstmt.executeUpdate();** 를 통해 해당 statement를 실행한 결과를 db에 저장된 테이블을 갱신해준다.
***
# joinAction.jsp
```jsp
<jsp:useBean id="user" class="user.User" scope="page" />
<jsp:setProperty name="user" property="userID" />
<jsp:setProperty name="user" property="userPassword" />
<jsp:setProperty name="user" property="userName" />
<jsp:setProperty name="user" property="userGender" />
<jsp:setProperty name="user" property="userEmail" />
```
join.jsp에서 넘어오는 정보 5개를 모두 받아오기위해 자바빈즈를 사용하였다.<br>
id= 자바빈을 사용할 아이디이름<br>
class= 해당 정보가 들어있는 자바빈즈 class 이름<br>
scope="page" 현재 페이지에만 적용되게 설정<br>
name= 자바빈의 id<br>
property= 정보를 받아올 해당 이름 **주의점:** property의 이름은 해당 폼이 있는 곳의 이름과 자바빈 클래스내의 변수 이름들이 같아야한다.<br>
```jsp
UserDAO userDAO = new UserDAO();
	int result = userDAO.join(user);
	if (result == -1){
		PrintWriter script = response.getWriter();
		script.println("<script>");
		script.println("alert('이미 존재하는 아이디입니다.')");
		script.println("history.back()");
		script.println("</script>");
	}
	else {
		PrintWriter script = response.getWriter();
		session.setAttribute("userID",user.getUserID());
		script.println("<script>");
		script.println("location.href = 'main.jsp'");
		script.println("</script>");
	}
	}
```
**int result = userDAO.join(user);** 여기서 user는 해당 회원의 정보를 넘겨 받은 자바빈의 id 즉 user.User.java의 객체<br>
result == -1 일때는 UserDAO.java에서 봤듯이 데이터베이스 오류이다. 이 경우에는 userID가 primary key이기 때문에 동일한 ID로 가입을<br>
시도했을때에 해당한다.<br>
script.println("location.href = 'main.jsp'"); 회원가입이 정상적으로 진행 되었을때 main.jsp로 이동

