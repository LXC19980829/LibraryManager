#### 运行环境
- JDK(32位jdk) access是只支持32位（x86)所以必须在32位jdk下运行
- windows mac linux ....
- 数据库（access数据库）


#### 使用技术
- Java awt 非 swing
- access 数据库
- 数据库驱动 sun.jdbc.odbc.JdbcOdbcDriver

#### 连接数据库代码
```java
public class DbOp{
	private static String driver="sun.jdbc.odbc.JdbcOdbcDriver";
  /**
  *jdbc:odbc:bookdb";
  *数据库名称是 bookdb
  *
  **/
	private static String url="jdbc:odbc:bookdb";
	private static Connection con=null;
	private DbOp(){
		try{
			if(con==null){
				Class.forName(driver);
				con=DriverManager.getConnection(url);
			}
		}catch(Exception e){
			JOptionPane.showMessageDialog(null,"数据库未能打开！");
			System.out.println(e.getMessage());
		}
	}
```
  
#### 入口类
 - Login.java 点击登陆按钮的代码
 ```java
 	public void sureActionListener(ActionEvent le){
		String user=text_user.getText();//从文本框得到输入的用户名和密码
		String pass=text_pass.getText();
		String is_admin="";
		if(user.equals("")||pass.equals("")){
			JOptionPane.showMessageDialog(null,"密码不能为空，请输入密码");
			return;
		}
		try{
    /**
    *数据库核对密码是否正确，正确就登陆成功
    *
    */
			String sql="select * from user where username="+"'"+user+"'"+"and password="+"'"+pass+"'";
			ResultSet rs=DbOp.executeQuery(sql);
				if(rs.next()){
					is_admin=rs.getString("is_admin");
				}else{
					JOptionPane.showMessageDialog(null,"Wrong that is UserNmae or Password ");
					return;
				}
			GlobalVar.login_user=user;				
			ShowMain show=new ShowMain();
			show.setRights(is_admin);
			System.out.println("Successed");
			dispose();
		}catch(SQLException e){
			JOptionPane.showMessageDialog(null,"the wrong from information");
		}
	}
 ```
  #### access 数据源配置
  - 将uboger/LibraryManager下的“图书管理.mdb” 下载到本地电脑
  - 建议不要放在有中文名的路径下
  - 下载方法略过
  - 打开>控制面板>所有控制面板项>管理工具>ODBC数据源(32位）进行数据源配置
  - 用户DNS>添加>选择 Driver do Microsoft Access(*.mdb)项
  - 对话框填写数据源名为 “bookdb”
  - 选择“图书管理.mdb”作为数据源>确定>确定>确定
  
  

  
